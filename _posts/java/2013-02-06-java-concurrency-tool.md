---
layout: post
catalog: true
title: "java常用并发工具介绍"
description: ""
category: java
subhead: ''
tags: [java,  concurrency]

---

本文主要介绍的工具包括:

* ####CountDownLatch
* ####Semaphore
* ####CyclicBarrier
* ####Exchanger 

### CountDownLatch
CountDownLatch可以使一个或多个线程等待一组事件发生。在CountDownLatch内部维护一个计数器(被初始化为一个正整数)，表示需要等待事件的数量。`countDown()`方法减少一个事件数量，`await()`将等待直到计数器为零的时候，才继续执行await后面的代码。如果计数器不为零，那么await将一直会阻塞等待直到计数器为零，或者阻赛线程中断/超时。

    @Test
    public void test() throws InterruptedException {
        // thread number
        final int threadNum = Runtime.getRuntime().availableProcessors() + 1;
        // start event
        final CountDownLatch startEvent = new CountDownLatch(1);
        // finish event
        final CountDownLatch finishEvent = new CountDownLatch(threadNum);

        for (int i = 0; i < threadNum; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        // await for start
                        startEvent.await();
                        System.out.println(Thread.currentThread() + " start at : " + System.currentTimeMillis());
                        // current thread finish
                        finishEvent.countDown();
                    } catch (InterruptedException ignore) {

                    }
                }
            }).start();

            // sleep 0.5ms
            TimeUnit.MILLISECONDS.sleep(500);
        }

        long startTime = System.currentTimeMillis();

        startEvent.countDown();
        // wait for all thread finish
        finishEvent.await();
        System.out.println("total finish cost : " + (System.currentTimeMillis() - startTime) + "ms");

    }
这个例子展示了如何在同一时间启动`threadNum`个线程，并且这`threadNum`个线程都完成后，记录执行结果。`startEvent.await()`将等待直到调用`startEvent.countDown()`。这是，所有线程在同一时间启动。当每个线程执行完毕的时候，会调用`finishEvent.countDown()`通知给主线程，`finishEvent.await()`将等待直到所有子线程都执行完毕。   
    
打印结果：

    Thread[Thread-1,5,main] start at : 1359782125125
    Thread[Thread-8,5,main] start at : 1359782125125
    Thread[Thread-7,5,main] start at : 1359782125125
    Thread[Thread-6,5,main] start at : 1359782125125
    Thread[Thread-5,5,main] start at : 1359782125125
    Thread[Thread-3,5,main] start at : 1359782125125
    Thread[Thread-0,5,main] start at : 1359782125125
    Thread[Thread-2,5,main] start at : 1359782125125
    Thread[Thread-4,5,main] start at : 1359782125125
    total finish cost : 1ms

### Semaphore 
Semaphore在内部持有一个虚拟的许可组(初始化的时候可以设置虚拟组的数量)，当执行某个操作的时候，调用`acquire`获得许可，在操作执行完成后调用`release`释放许可。如果没有许可可用，那么`acquire`方法会一直阻赛直到有许可可用为止，或者执行获取许可的线程终端或阻赛。

Semaphore可以用来控制某种资源的使用数量，或者同时使用特定资源的数量。利用这个特性，可以实现某种资源的资源池或者对容器实加边界。

    @Test
    public void test() throws InterruptedException {

        final BoundedList<Integer> list = new BoundedList<>(5);

        new Thread(new Runnable() {
            @Override
            public void run() {
                int index = 0;
                while (true) {
                    try {
                        list.add(index++);
                        System.out.println(System.currentTimeMillis() + " add " + index);
                    } catch (InterruptedException ignore) {

                    }
                }
            }
        }).start();

        TimeUnit.SECONDS.sleep(1);

        new Thread(new Runnable() {
            @Override
            public void run() {
                int index = 0;
                while (true) {
                    try {
                        list.remove(index++);
                        System.out.println(System.currentTimeMillis() + " remove " + index);
                        TimeUnit.MILLISECONDS.sleep(500);
                    } catch (InterruptedException ignore) {
                    }
                }
            }
        }).start();
        Thread.currentThread().join();
    }

    static class BoundedList<T> {

        private final List<T> list;
        private final Semaphore semaphore;

        public BoundedList(int bound) {
            this.list = new ArrayList<>();
            semaphore = new Semaphore(bound);
        }

        public boolean add(T o) throws InterruptedException {
            semaphore.acquire();
            boolean added = false;
            try {
                added = list.add(o);
                return added;
            } finally {
                if (!added) {
                    semaphore.release();
                }
            }
        }

        public boolean remove(T o) {
            boolean removed = list.remove(o);
            if (removed) {
                semaphore.release();
            }
            return removed;
        }
    }        
    
这个例子展示了一个`带边界的List`，当向集合中添加元素的时候，首先获取许可。`如果添加失败了，那么释放许可`。当删除集合中的元素的时候，如果删除成功，释放一个许可。这样就能保证集合中的元素都是获得许可后才添加进来的，从而保证了集合的边界。      

打印结果：

    1359787233784 add 1
    1359787233784 add 2
    1359787233784 add 3
    1359787233784 add 4
    1359787233784 add 5
    1359787234787 add 6
    1359787234787 remove 1
    1359787235288 remove 2
    1359787235288 add 7
    1359787235789 remove 3
    1359787235789 add 8
    1359787236290 remove 4
    1359787236290 add 9
    ….
在这个例子中，生产者向集合中添加元素，消费者删除元素，因为生产者的速度大于消费者，所以当集合中元素等于5的时候，就必须等待消费者删除一个元素后才能再继续添加，从打印结果可以看出这点。

### CyclicBarrier
CyclicBarrier和CountDownLatch有些类似，它阻塞一组线程直到某个事件发生。可以把CyclicBarrier理解成一个障碍，当所有线程都到达这个"障碍"的时候，才能继续下个事件。如果所有线程到达barrier处，barrier打开释放所有线程，并且barrier可以继续使用。如果`await`方法超时，或者被中断，那么认为barrier被打破，所有在await上阻塞的线程都将抛出`BrokenBarrierException`。

    @Test
    public void test() throws InterruptedException {

        ExecutorService exec = Executors.newFixedThreadPool(Runtime.getRuntime()
                .availableProcessors() + 1);

        final int gate_threshold = 4;
        final CyclicBarrier gate = new CyclicBarrier(gate_threshold, new Runnable() {
            @Override
            public void run() {
                System.out.println("4 threads arrived, gate open...");
            }
        });

        while (true) {
            TimeUnit.MILLISECONDS.sleep((long) (Math.random() * 1000));
            exec.execute(new Runnable() {
                @Override
                public void run() {
                    System.out.println(Thread.currentThread() + " arrived");
                    try {
                        gate.await();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    } catch (BrokenBarrierException e) {
                        e.printStackTrace();
                    }
                }
            });
        }
    }


这个例子假设有一堆线程到达`gate`处，每当到达`gate`处的线程数达到`gate_threshold`时,`gate`打开释放这些线程并进入下次一次循环。


### Exchanger
Exchanger提供两个线程以线程安全的形式交换数据，exchange等待另一个线程到达exchange方法，然后把数据给另一个线程并且接收另一个线程交换过来的数据。


    @Test
    public void test() throws InterruptedException {
        
        final int size =10;
        final Exchanger<List<Integer>> exchanger = new Exchanger<>();
        final List<Integer> emptyList = new ArrayList<>();
        final List<Integer> fullList = new ArrayList<>();
        
        Thread producer = new Thread(new Runnable() {
            @Override
            public void run() {
                List<Integer> currentList = emptyList;
                while (true){
                   if(currentList.size()<size){
                       int num = (int)( Math.random() * 10000);
                       currentList.add(num);
                       System.out.println("producer : " + num);
                       try {
                           TimeUnit.MILLISECONDS.sleep((long) (Math.random()*1000));
                       } catch (InterruptedException ignore) {

                       }
                   }else {
                       try {
                           System.out.println("producer : list full wait exchange");
                           currentList =  exchanger.exchange(currentList);
                           System.out.println("producer : exchanged");
                       } catch (InterruptedException ignore) {
                           
                       }
                   }
                }
            }
        });

        Thread consumer = new Thread(new Runnable() {
            @Override
            public void run() {
                List<Integer> currentList = fullList;
                while (true){
                    if(currentList.size()!=0){
                        Integer remove = currentList.remove(0);
                        System.out.println("consumer : " + remove);
                        try {
                            TimeUnit.MILLISECONDS.sleep((long) (Math.random()*1000));
                        } catch (InterruptedException ignore) {
                        }
                    }else {
                        try {
                            System.out.println("consumer : list empty wait exchange");
                            currentList =  exchanger.exchange(currentList);
                            System.out.println("consumer : exchanged");
                        } catch (InterruptedException ignore) {

                        }
                    }
                }
            }
        });
        producer.start();
        consumer.start();
        producer.join();
        consumer.join();
    }
这个例子展示了一个生产者/消费者的例子。当生产者填慢list后，等待交换。同样当消费者消耗完list，也等待交换。




