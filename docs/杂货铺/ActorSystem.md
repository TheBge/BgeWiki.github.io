

首先通过descriptor name将对应的实现implementation和接口interface绑定起来，通过ActorRegistrant模板类构造函数构造对象。

-> ActorSystem::Register(this);

-> ActorSystemImpl::GetInstance()->Register(creator);

ActorSystemImpl是一个单例类，维护一个InterfaceToMetaMap，key和value分别是type_index/InterfaceMeta

InterfaceMeta维护一个ActorCreatorVector与KeyToActorMap。

Register()将typeid(InterfaceType)



```c++
void CallbackImplement::EnableCallback(
)
{
    m_callbackId = GetReasonId();
    m_registeredFunction = CALLBACK_add_function(m_callbackId, CallbackInternal, this);
}

void CallbackImplement::CallbackInternal(
    int             cbReason,
    const void* cbData,
    void* userData)
{
    CallbackImplement* callbackImpl = reinterpret_cast<CallbackImplement*>(userData);
    callbackImpl->Callback(cbData);
}
```

