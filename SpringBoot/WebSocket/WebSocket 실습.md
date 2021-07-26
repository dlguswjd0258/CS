# :female_detective: WebSocket

## WebSocketConfig

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

  @Override
  public void registerStompEndpoints(StompEndpointRegistry registry) {
    registry.addEndpoint("/api/room").setAllowedOrigins("*").withSockJS();
  }

  @Override
  public void configureMessageBroker(MessageBrokerRegistry registry) {
    // publisher : message-handling methods로 라우팅됨
    registry.setApplicationDestinationPrefixes("/pub");
    // subscriber : topic으로 시작되는 메시지가 메세지브로커로 라우팅됨
    registry.enableSimpleBroker("/sub");
  }

  @Override
  public void configureClientInboundChannel(ChannelRegistration registration) {
    registration.interceptors(new MyChannelInterceptor());
  }
}
```

- addEndpoint()
  - end point 설정
  - 여러 end point를 설정할 수 있다.
  - `/api/room`으로 endpoint 설정
- 

