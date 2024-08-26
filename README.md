# dop
data oriented programming

Data oriented programming 책을 읽고 정리한 내용이에요.
이 [예제 파일](https://github.com/viebel/data-oriented-programming) 을 참고하여 코드를 정리했어요

### DOP 대 원칙

1. 행동(code)과 데이터를 분리해요.
   - OOP, FP 둘 중 어떤 형태로든 데이터와 행동이 잘 분리되어있으면 돼요.
   - **장점**
      - 행동이 다른 컨텍스트에서 쓰일 수 있어요.
        ```javascript
        class AuthorData {
           constructor (firstName, lastName, books) {
                 this.firstName = firstName;
                 this.lastName = lastName;
                 this.books = books;
           }
        }
     
        // 이런식으로 행동을 분리시킬 수 있음
         class NameCalculation {
           static fullName (data) { // data 는 아무 타입이나 들어올 수 있게 설계해야함 
              return data. firstName + " " + data. lastName;
           }
        }
     
        class UserData {
         constructor (firstName, lastName, email) {
             this.firstName = firstName;
             this.lastName = lastName;
             this.email = email;
            }
        }
        ```
      - 행동의 테스트가 독립적으로 변경되어요.
   - **비용**
     - 어떤 행위가 어떤 데이터에 접근 가능한지 확인이 어려워요
     - 패키지가 존재하지 않아요. (바운더리가 없다는 뜻일듯)
     - 시스템은 훨씬 더 방대한 양의 엔티티를 통해 생성돼요. ~ 행동과 데이터가 분리되면 2N 개의 클래스가 생성돼요.
   2. 특정한 class 가 아닌 generic 한 데이터 구조를 사용해요.
    
       - **설명**
         - class 대신 map 이나 list 등의 제네릭한 데이터 구조를 사용해요.
         - 대부분의 타입이 있는 언어들은 map 이나 list 로 표현이 가느앻요
         - 필요하다면 set, queue 도 사용할 수 있어요
         ```javascript
         // 이런식으로 map 으로 데이터를 표현해요
         function createAuthorData (firstName, lastName, books) {
           return {
             firstName: firstName,
             lastName: lastName,
             books: books,
           }
         }
         ```

      - **장점**
        - 특정한 유즈케이스에 대해 표현하는 방식에 대한 제약이 없어요.
          - 필요한 데이터를 직접 생산하거나 표현하면 돼요. 
        - 데이터 모델이 훨씬 유연해져요. (확장성) 
      - **단점**
        - 성능 이슈에 맞딱트릴 수 있어요. 
          - 일반적으로 class 같은건 컴파일러가 잘 최적화 해주지먄 generic 타입들은 최적화를 잘 못해줘요. 
          - 데이터를 가져올 때도 class 의 멤버를 가져올 때 보다 느려지고요.
        - 
3. 데이터를 immutable 하게 관리해요
   - **장점**
       - 모든 사람들이 데이터에 접근할 수 있어요
         - 데이터가 변경되지 않으면 데이터를 공유해도 안전해요.
       - 행동이 예측 가능해요
         - timeout 같은것을 통해 사이드 이펙트로 값이 예상 못한 타이밍에 변경될 가능성도 있음
         - 데이터가 변경 불가능하면 값이 변경되어도 새로운 값으로 설정되기에 문제가 없음
       - 빠르게 Equal 체크 가능
         - React 같은 케이스에서는 address 의 변경만 보고 렌더링을 할지 말지 결정할 수 있어요.
         - 이와 같이 빠르게 주소만 보고 데이터 체크가 가능해요
       - 동시성 문제가 자연스럽게 해결됨
         - 멀티스레드에서는 어차피 다른스레드로 넘어갈 때 변수가 복사되므로 쉽게 해결됨
   - **단점**
     - 이것 역시 메모리에 계속 객체를 복사하기에 성능 이슈가 있어요
     - 
4. 데이터 스키마와 표현을 분리해요


### 예제 코드 생성을 위한 주요 정책을 정리했어요

- DOP 에서는 데이터가 런타임에 추가되거나 삭제될 수 있어요.
  - 따라서 class, type 이 아닌 map 을 사용할거에요.
  - 책에서는 lodash 를 많이 활용하지만 kotlin 만으로도 데이터를 잘 핸들링할 수 있어요. 


### Database 모델

- Schema 모델링 및 데이터 저장
  RDS, noSQL 등의 데이터베이스가 있어요.
  특별히 JSON 같은거에 다 때려박진 않고, 필드들을 나눠서 저장해요.
  즉석으로 APP 에서 필드를 추가하거나 정의되지 않은 필드를 저장하거나 하기는 어려워요. ㅠ

- 데이터 쿼리
  Map 방식으로 데이터를 불러온 뒤 직접 어그리게이션을 해요.
