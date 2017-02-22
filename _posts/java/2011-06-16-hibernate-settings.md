---
layout: post
catalog: true
title: "hibernate属性配置"
description: ""
category: java
subhead: ''
tags: [hibernate,  java, framework]

---

这些属性有的时候很需要，但是记不住，所以做个备忘。
 
属性名：hibernate.ejb.classcache.&lt;classname&gt;

描述：指定缓存实体对象，&lt;classname&gt;为缓存类的全名，值为缓存类型，以逗号分隔。

示例如下：

    <property name='hibernate.ejb.classcache.com.fengmanfei.jpa.entity.Customer' value='read-write'>  
    
----    
属性名：hibernate.ejb.collectioncache.&lt;collectionrole&gt;

描述：指定集合实体类缓存，设置同上。&lt;collectionrole&gt;为集合类的全名，值为缓存类型，以逗号分隔。

示例如下：
  
    <property name='hibernate.ejb.collectioncache.com.fengmanfei.jpa.entity.Customer. orders' value='read-write , RegionName '/> 
     
##### 提示
读者若想了解更多的缓存设置，请参阅JBoss Cache的相关文档。

----

属性名：hibernate.ejb.cfgfile

描述：指定使用Hibernate配置文件中的配置。

示例如下：
  
    < property name='hibernate.ejb.cfgfile' value='/com/fengmanfei/jpa/hibernate.cfg.xml'/ > 
    
----    
     
属性名：hibernate.archieve.autodetection

描述：创建Entity Manager时搜索文件的类型，多个值之间用逗号分隔。

可选值：
·class：.class类文件。
·hbm：Hibernate 配置文件。

默认两个都搜索。

示例如下：
  
    <property name='hibernate.archive.autodetection' value='class,hbm'/>  
    
----    
    
属性名：hibernate.ejb.interceptor

描述：自定义拦截器类名，拦截器必须实现了org.hibernate.Interceptor接口，并且有无参的构造方法。

示例如下：

    <property name='hibernate.ejb.interceptor' 'value='com.fengmanfei.jpa.interceptor.MyInterceptor'/>  
 
----    
    
属性名：hibernate.ejb.naming_strategy

描述：设置注释命名策略。

可选值：
·EJB3NamingStrategy（默认）：EJB3规范的命名实现。
·DefaultComponentSafeNamingStrategy：在默认的EJB3NamingStrategy上进行了扩展，允许在同一实体中使用两个同类型的嵌入对象而无须额外的声明。

示例如下：
  
    <property name=' hibernate.ejb.naming_strategy ' value=' DefaultComponentSafeNamingStrategy '/>  
    
----    
    
属性名：hibernate.ejb.event.&lt;eventtype&gt;

描 述：配置事件监听器，其中&lt;eventtype&gt;为监听的事件类型，事件类型如下表中列举所示。而值则为具体监听器类的全名，如果有多 个则使用逗号分隔。自定义拦截器类，拦截器必须实现了org.hibernate.Interceptor接口，并且有无参的构造方法，在JPA的环境 中，尽量继承下表中的时间监听器类。

##### 可选的监听事件类型
 
事件类型  | 监听器类
:------ | :--------
flush | org.hibernate.ejb.event.EJB3FlushEventListener
auto-flush | org.hibernate.ejb.event.EJB3AutoFlushEventListener
delete | org.hibernate.ejb.event.EJB3DeleteEventListener
flush-entity | org.hibernate.ejb.event.EJB3FlushEntityEventListener
merge | org.hibernate.ejb.event.EJB3MergeEventListener
create | org.hibernate.ejb.event.EJB3PersistEventListener
create-onflush | org.hibernate.ejb.event.EJB3PersistOnFlushEventListener
save | org.hibernate.ejb.event.EJB3SaveEventListener
save-update | org.hibernate.ejb.event.EJB3SaveOrUpdateEventListener
pre-insert | org.hibernate.secure.JACCPreInsertEventListener,org.hibernate.valitator.event.ValidateEventListener 
pre-update | org.hibernate.secure.JACCPreUpdateEventListener,org.hibernate.valitator.event.ValidateEventListener 
pre-delete | org.hibernate.secure.JACCPreDeleteEventListener
pre-load  | org.hibernate.secure.JACCPreLoadEventListener
post-delete | org.hibernate.ejb.event.EJB3PostDeleteEventListener
post-insert | org.hibernate.ejb.event.EJB3PostInsertEventListener
post-load | org.hibernate.ejb.event.EJB3PostLoadEventListener
post-update | org.hibernate.ejb.event.EJB3PostUpdateEventListener

示例如下：
 
    <property name='hibernate.ejb.event.create' value='com.fengmanfei.listener.CreateListener' />  

其中，CreateListener继承org.hibernate.ejb.event.EJB3PersistEventListener类，代码如下所示。
   
    import org.hibernate.HibernateException;  
    import org.hibernate.ejb.event.EJB3PersistEventListener;  
    import org.hibernate.event.PersistEvent;  
    public class CreateListener extends EJB3PersistEventListener {  
    // 覆盖父类中的方法  
    @Override  
        public void onPersist(PersistEvent event) throws HibernateException {  
        super.onPersist(event);  
        //代码处理  
        }  
    }  
    
----    
 
属性名：hibernate.ejb.use_class_enhancer

描述：是否启用应用服务器扩展类。

可选值：
·true：启用扩展类。
·false（默认）：禁用扩展类。

示例如下：
  
    <property name=' hibernate.ejb.use_class_enhancer ' value=' true”/>  
 
----    
    
属性名：hibernate.ejb.discard_pc_on_close

描述：是否在执行clear()时脱离持久化上下文。

可选值：
·true：执行clear()时脱离持久化上下文。
·false（默认）：执行clear()时不脱离持久化上下文。

示例如下：
 
    <property name=' hibernate.ejb.discard_pc_on_close ' value=' true”/>  
 
 

