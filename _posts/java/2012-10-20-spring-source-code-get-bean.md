---
layout: post
title: "Spring源码阅读——获得bean"
description: ""
category: java
subhead: ''
tags: [spring,  java, source]

---


已XmlWebApplicationContext为例,从getBean(String name)方法开始，读一下Spring是怎么通过名字获得bean的。
其他方式类似这个过程。

这个是`XmlWebApplicationContext`的类继承体系：

![image](http://i1298.photobucket.com/albums/ag53/lichengwu/get_bean_zps8050af78.png)

首先，getBean(String name)是在BeanFactory接口中定义的，而在AbstractApplicationContext中实现：
 
    public Object getBean(String name) throws BeansException {  
        return getBeanFactory().getBean(name);  
    } 
     
AbstractApplicationContext获得BeanFactory代理，然后用代理获得bean。这个代理的实现的getBean(String name)是在AbstractBeanFactory中实现的：
  
    public Object getBean(String name) throws BeansException {  
        return doGetBean(name, null, null, false);  
    }   
    
这里的doGenBean才是真正获得bean的地方，下面逐步分析这个方法：

 
    protected <T> T doGetBean(  
            final String name, final Class<T> requiredType, final Object[] args, boolean typeCheckOnly)  
            throws BeansException {  
  
        final String beanName = transformedBeanName(name);  
        Object bean;  
  
        // Eagerly check singleton cache for manually registered singletons.  
        Object sharedInstance = getSingleton(beanName);  
        if (sharedInstance != null && args == null) {  
            if (logger.isDebugEnabled()) {  
                if (isSingletonCurrentlyInCreation(beanName)) {  
                    logger.debug("Returning eagerly cached instance of singleton bean '" + beanName +  
                            "' that is not fully initialized yet - a consequence of a circular reference");  
                }  
                else {  
                    logger.debug("Returning cached instance of singleton bean '" + beanName + "'");  
                }  
            }  
            bean = getObjectForBeanInstance(sharedInstance, name, beanName, null);  
        }  
  
        else {  
            // Fail if we're already creating this bean instance:  
            // We're assumably within a circular reference.  
            if (isPrototypeCurrentlyInCreation(beanName)) {  
                throw new BeanCurrentlyInCreationException(beanName);  
            }  
  
            // Check if bean definition exists in this factory.  
            BeanFactory parentBeanFactory = getParentBeanFactory();  
            if (parentBeanFactory != null && !containsBeanDefinition(beanName)) {  
                // Not found -> check parent.  
                String nameToLookup = originalBeanName(name);  
                if (args != null) {  
                    // Delegation to parent with explicit args.  
                    return (T) parentBeanFactory.getBean(nameToLookup, args);  
                }  
                else {  
                    // No args -> delegate to standard getBean method.  
                    return parentBeanFactory.getBean(nameToLookup, requiredType);  
                }  
            }  
  
            if (!typeCheckOnly) {  
                markBeanAsCreated(beanName);  
            }  
  
            final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);  
            checkMergedBeanDefinition(mbd, beanName, args);  
  
            // Guarantee initialization of beans that the current bean depends on.  
            String[] dependsOn = mbd.getDependsOn();  
            if (dependsOn != null) {  
                for (String dependsOnBean : dependsOn) {  
                    getBean(dependsOnBean);  
                    registerDependentBean(dependsOnBean, beanName);  
                }  
            }  
  
            // Create bean instance.  
            if (mbd.isSingleton()) {  
                sharedInstance = getSingleton(beanName, new ObjectFactory() {  
                    public Object getObject() throws BeansException {  
                        try {  
                            return createBean(beanName, mbd, args);  
                        }  
                        catch (BeansException ex) {  
                            // Explicitly remove instance from singleton cache: It might have been put there  
                            // eagerly by the creation process, to allow for circular reference resolution.  
                            // Also remove any beans that received a temporary reference to the bean.  
                            destroySingleton(beanName);  
                            throw ex;  
                        }  
                    }  
                });  
                bean = getObjectForBeanInstance(sharedInstance, name, beanName, mbd);  
            }  
  
            else if (mbd.isPrototype()) {  
                // It's a prototype -> create a new instance.  
                Object prototypeInstance = null;  
                try {  
                    beforePrototypeCreation(beanName);  
                    prototypeInstance = createBean(beanName, mbd, args);  
                }  
                finally {  
                    afterPrototypeCreation(beanName);  
                }  
                bean = getObjectForBeanInstance(prototypeInstance, name, beanName, mbd);  
            }  
  
            else {  
                String scopeName = mbd.getScope();  
                final Scope scope = this.scopes.get(scopeName);  
                if (scope == null) {  
                    throw new IllegalStateException("No Scope registered for scope '" + scopeName + "'");  
                }  
                try {  
                    Object scopedInstance = scope.get(beanName, new ObjectFactory() {  
                        public Object getObject() throws BeansException {  
                            beforePrototypeCreation(beanName);  
                            try {  
                                return createBean(beanName, mbd, args);  
                            }  
                            finally {  
                                afterPrototypeCreation(beanName);  
                            }  
                        }  
                    });  
                    bean = getObjectForBeanInstance(scopedInstance, name, beanName, mbd);  
                }  
                catch (IllegalStateException ex) {  
                    throw new BeanCreationException(beanName,  
                            "Scope '" + scopeName + "' is not active for the current thread; " +  
                            "consider defining a scoped proxy for this bean if you intend to refer to it from a singleton",  
                            ex);  
                }  
            }  
        }  
  
        // Check if required type matches the type of the actual bean instance.  
        if (requiredType != null && bean != null && !requiredType.isAssignableFrom(bean.getClass())) {  
            throw new BeanNotOfRequiredTypeException(name, requiredType, bean.getClass());  
        }  
        return (T) bean;  
    }  
    
首先从缓存查找单例：
 
    Object sharedInstance = getSingleton(beanName);  
        if (sharedInstance != null && args == null) {  
            if (logger.isDebugEnabled()) {  
                if (isSingletonCurrentlyInCreation(beanName)) {  
                    logger.debug("Returning eagerly cached instance of singleton bean '" + beanName +  
                            "' that is not fully initialized yet - a consequence of a circular reference");  
                }  
                else {  
                    logger.debug("Returning cached instance of singleton bean '" + beanName + "'");  
                }  
            }  
            bean = getObjectForBeanInstance(sharedInstance, name, beanName, null);  
        } 
         
如果能从缓存中获得已缓存的单例bean，并且参数args(只能在创建prototype时使用)为空，那么直接返回缓存中的bean，否则继续创建bean：

  
    BeanFactory parentBeanFactory = getParentBeanFactory();  
            if (parentBeanFactory != null && !containsBeanDefinition(beanName)) {  
                // Not found -> check parent.  
                String nameToLookup = originalBeanName(name);  
                if (args != null) {  
                    // Delegation to parent with explicit args.  
                    return (T) parentBeanFactory.getBean(nameToLookup, args);  
                }  
                else {  
                    // No args -> delegate to standard getBean method.  
                    return parentBeanFactory.getBean(nameToLookup, requiredType);  
                }  
            }  
如果能AbstractBeanFactory存在父级BeanFactory，并且当前AbstractBeanFactory不存在bean的定义(BeanDefinition),就把创建bean交给父级Beanfactory。
 
    BeanFactory parentBeanFactory = getParentBeanFactory();  
        if (parentBeanFactory != null && !containsBeanDefinition(beanName)) {  
            // Not found -> check parent.  
            String nameToLookup = originalBeanName(name);  
            if (args != null) {  
                // Delegation to parent with explicit args.  
                return (T) parentBeanFactory.getBean(nameToLookup, args);  
            }  
            else {  
                // No args -> delegate to standard getBean method.  
                return parentBeanFactory.getBean(nameToLookup, requiredType);  
            }  
        }  
如果父级BeanFactory不存在或者AbstractBeanFactory存在当前bean的定义，那就从BeanDefinition中创建bean。
在用BeanDefinition创建bean过程中首先要递归的解决依赖：
 
    final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);  
            checkMergedBeanDefinition(mbd, beanName, args);  
  
            // Guarantee initialization of beans that the current bean depends on.  
            String[] dependsOn = mbd.getDependsOn();  
            if (dependsOn != null) {  
                for (String dependsOnBean : dependsOn) {  
                    getBean(dependsOnBean);  
                    registerDependentBean(dependsOnBean, beanName);  
                }  
            }  
处理完依赖后，开始创建bean实例。如果bean是单例，则创建这个单例并放入缓存：

    if (mbd.isSingleton()) {  
                sharedInstance = getSingleton(beanName, new ObjectFactory() {  
                    public Object getObject() throws BeansException {  
                        try {  
                            return createBean(beanName, mbd, args);  
                        }  
                        catch (BeansException ex) {  
                            // Explicitly remove instance from singleton cache: It might have been put there  
                            // eagerly by the creation process, to allow for circular reference resolution.  
                            // Also remove any beans that received a temporary reference to the bean.  
                            destroySingleton(beanName);  
                            throw ex;  
                        }  
                    }  
                });  
                bean = getObjectForBeanInstance(sharedInstance, name, beanName, mbd);  
            }  
创建bean的实际动作是在createBean方法中，这个方法在AbstractAutowireCapableBeanFactory中实现：
 
    protected Object createBean(final String beanName, final RootBeanDefinition mbd, final Object[] args)  
            throws BeanCreationException {  
  
        if (logger.isDebugEnabled()) {  
            logger.debug("Creating instance of bean '" + beanName + "'");  
        }  
        // Make sure bean class is actually resolved at this point.  
        resolveBeanClass(mbd, beanName);  
  
        // Prepare method overrides.  
        try {  
            mbd.prepareMethodOverrides();  
        }  
        catch (BeanDefinitionValidationException ex) {  
            throw new BeanDefinitionStoreException(mbd.getResourceDescription(),  
                    beanName, "Validation of method overrides failed", ex);  
        }  
  
        try {  
            // Give BeanPostProcessors a chance to return a proxy instead of the target bean instance.  
            Object bean = resolveBeforeInstantiation(beanName, mbd);  
            if (bean != null) {  
                return bean;  
            }  
        }  
        catch (Throwable ex) {  
            throw new BeanCreationException(mbd.getResourceDescription(), beanName,  
                    "BeanPostProcessor before instantiation of bean failed", ex);  
        }  
  
        Object beanInstance = doCreateBean(beanName, mbd, args);  
        if (logger.isDebugEnabled()) {  
            logger.debug("Finished creating instance of bean '" + beanName + "'");  
        }  
        return beanInstance;  
    }  
createBean方法首先验证要创建bean的Class，然后准备要重写的方法。

接下来，将解析是否创建一个bean的代理而不是正在创建这个bean：

    Object bean = resolveBeforeInstantiation(beanName, mbd);  
在AbstractAutowireCapableBeanFactory是创建代理具体实现:


    protected Object resolveBeforeInstantiation(String beanName, RootBeanDefinition mbd) {  
        Object bean = null;  
        if (!Boolean.FALSE.equals(mbd.beforeInstantiationResolved)) {  
            // Make sure bean class is actually resolved at this point.  
            if (mbd.hasBeanClass() && !mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {  
                bean = applyBeanPostProcessorsBeforeInstantiation(mbd.getBeanClass(), beanName);  
                if (bean != null) {  
                    bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);  
                }  
            }  
            mbd.beforeInstantiationResolved = (bean != null);  
        }  
        return bean;  
    }  
如果代理不为空，就返回代理；否则创建bean：
 
    Object beanInstance = doCreateBean(beanName, mbd, args);  
 
创建过程是在AbstractAutowireCapableBeanFactory中实现的：

    protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final Object[] args) {  
        // Instantiate the bean.  
        BeanWrapper instanceWrapper = null;  
        if (mbd.isSingleton()) {  
            instanceWrapper = this.factoryBeanInstanceCache.remove(beanName);  
        }  
        if (instanceWrapper == null) {  
            instanceWrapper = createBeanInstance(beanName, mbd, args);  
        }  
        final Object bean = (instanceWrapper != null ? instanceWrapper.getWrappedInstance() : null);  
        Class beanType = (instanceWrapper != null ? instanceWrapper.getWrappedClass() : null);  
  
        // Allow post-processors to modify the merged bean definition.  
        synchronized (mbd.postProcessingLock) {  
            if (!mbd.postProcessed) {  
                applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);  
                mbd.postProcessed = true;  
            }  
        }  
  
        // Eagerly cache singletons to be able to resolve circular references  
        // even when triggered by lifecycle interfaces like BeanFactoryAware.  
        boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences &&  
                isSingletonCurrentlyInCreation(beanName));  
        if (earlySingletonExposure) {  
            if (logger.isDebugEnabled()) {  
                logger.debug("Eagerly caching bean '" + beanName +  
                        "' to allow for resolving potential circular references");  
            }  
            addSingletonFactory(beanName, new ObjectFactory() {  
                public Object getObject() throws BeansException {  
                    return getEarlyBeanReference(beanName, mbd, bean);  
                }  
            });  
        }  
  
        // Initialize the bean instance.  
        Object exposedObject = bean;  
        try {  
            populateBean(beanName, mbd, instanceWrapper);  
            if (exposedObject != null) {  
                exposedObject = initializeBean(beanName, exposedObject, mbd);  
            }  
        }  
        catch (Throwable ex) {  
            if (ex instanceof BeanCreationException && beanName.equals(((BeanCreationException) ex).getBeanName())) {  
                throw (BeanCreationException) ex;  
            }  
            else {  
                throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Initialization of bean failed", ex);  
            }  
        }  
  
        if (earlySingletonExposure) {  
            Object earlySingletonReference = getSingleton(beanName, false);  
            if (earlySingletonReference != null) {  
                if (exposedObject == bean) {  
                    exposedObject = earlySingletonReference;  
                }  
                else if (!this.allowRawInjectionDespiteWrapping && hasDependentBean(beanName)) {  
                    String[] dependentBeans = getDependentBeans(beanName);  
                    Set<String> actualDependentBeans = new LinkedHashSet<String>(dependentBeans.length);  
                    for (String dependentBean : dependentBeans) {  
                        if (!removeSingletonIfCreatedForTypeCheckOnly(dependentBean)) {  
                            actualDependentBeans.add(dependentBean);  
                        }  
                    }  
                    if (!actualDependentBeans.isEmpty()) {  
                        throw new BeanCurrentlyInCreationException(beanName,  
                                "Bean with name '" + beanName + "' has been injected into other beans [" +  
                                StringUtils.collectionToCommaDelimitedString(actualDependentBeans) +  
                                "] in its raw version as part of a circular reference, but has eventually been " +  
                                "wrapped. This means that said other beans do not use the final version of the " +  
                                "bean. This is often the result of over-eager type matching - consider using " +  
                                "'getBeanNamesOfType' with the 'allowEagerInit' flag turned off, for example.");  
                    }  
                }  
            }  
        }  
  
        // Register bean as disposable.  
        try {  
            registerDisposableBeanIfNecessary(beanName, bean, mbd);  
        }  
        catch (BeanDefinitionValidationException ex) {  
            throw new BeanCreationException(mbd.getResourceDescription(), beanName, "Invalid destruction signature", ex);  
        }  
  
        return exposedObject;  
    }   
    
doCreateBean方法主要填充bean的属性，处理依赖关系，处理post-processors，然后生成Constructor，根据测试采用jdk的反射或者CGLIB(默认)生成bean，关于这个过程，有时间再详细说，先欠着吧，这个编辑器太不好用了，真的写写不下去了...


---
{% include JB/setup %}