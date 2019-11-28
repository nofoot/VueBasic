#Life cycle of vue instance

Vue의 라이프 사이클은 크게 아래처럼 나눌 수 있다.

라이프 사이클 훅(LifeCycle hooks)은 Vue 오브젝트 전체 라이프사이클의 
특정 단계에서 실행되는 정의된 메소드 입니다.

Creation -> Mounting -> Updating -> Destruction  

1) Creation : (beforeCreate Created)
    - 이 단계에서 실행되는 훅들이 라이프 사이클 과정에서 가장 처음으로 실행된다
    - 이 단계는 컴포넌트가 DOM에 추가되기 전이다.
    - 서버 렌더링에서도 지원되는 훅
    - 아직 컴포넌트가 DOM에 추가되기 전이기 때문에 DOM의 접근 , this.$el 사용불가 
    
    1-1) beforeCreat
        - 모든 훅중에 가장 먼저 실행되는 훅
        - data와 events(vm.$on, vm.$once, vm.$off, vm.$emit)가 세팅되지 않은 시점이므로 접근시 에러 
    
    1-2) create
        - data와 events가 활성화 되어 접근 가능
        - 여전히 템플릿과 가상돔은 마운트 및 렌더링되지 않은 상태

2) Mounting : DOM 삽입 단계 (beforeMount, mounted)
    - 초기 렌더링 직전에 컴포넌트에 직접 접근할 수 있다.
    - 서버 렌더링에서는 지원하지 않는다.
    - 초기 렌더링 직전에 DOM을 변경하고자 한다면 이 단계를 활용할 수 있다.
    - 컴포넌트 초기에 세팅되어야 할 데이터 페치는 created단계를 사용하는 것이 좋다.
    
    2-1) beforeMount
        - Template과 Render 함수들이 컴파일 된 후에 첫 렌더링이 일어나기 직전에 실행
        - 대부분의 경우에 사용하지 않는 것이 좋다
        - 서버사이드 렌더링시에는 호출되지 않는다.

    2-2) mounted
        - Component, Template, 렌더링된 돔에 접근할 수 있다.
        - 모든 하위 Component가 마운트 된 상태를 보장하지는 않는다.
        - 서버렌더링에서는 호출되지 않는다 
        - 부모와 자식관계의 컴포넌트에서는 자식의 mount가 먼저 발생된다.

3) Updating : Diff 및 재 렌더링 단계 (beforeUpdate, update)
    - 컴포넌트에서 사용되는 반응형 속성들이 변경되거나 재 렌더링되면 발생된다.
    - 서버렌더링에서는 호출되지 않는다.
    
    3-1) beforeUpdate
        - 컴포넌트의 데이터가 변경되어 업데이트 사이클이 시작될때 실행 된다.
        - DOM이 재 렌더링되고 패치되기 직전에 실행된다.
        - 재 렌더링 전의 새 상태의 데이터를 얻을 수 있고 더 많은 변경이 가능하다.
        - 이 변경으로 재 렌더링되지 않는다

    3-2) updated
        - 컴포넌트의 데이터가 변경되어 재 렌더링이 일어나 후에 실행된다.
        - DOM이 업데이트 완료된 상태이기 때문에 DOM의 종속적인 연산을 할수 있다.
        - 여기서 updated 훅이 실행될 변경이 있는 경우 무한루프에 빠질수 있다.
        - 모든 자식 컴포넌트의 렌더링 상태를 보장하지 않는다.

4) Destruction : 해체단계(beforeDestroy, destroyed)
   
    4-1) beforeDestroy
        - 해체(뷰인스턴스 제거)되기 전에 호출된다.
        - 컴포넌트는 원래 모습과 모든 기능들을 그대로 가지고 있다.
        - 이벤트 리스너를 해제하거나 reactive subscription을 제거하고자 한다면 이 훅에서 처리하는것이 적합
    
    4-2) destroyed
        - 해체(뷰 인스턴스 제거)된 후에 호출
        - Vue 인스턴스의 모든 디렉티브가 바인딩 해제 되고 모든 이벤트 리스너가 제거된다
        - 모든 하위 Vue 인스턴스도 삭제된다.
        - 서버 렌더링시 호출되지 않는다.







