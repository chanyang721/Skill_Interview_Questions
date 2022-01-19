## MySQL의 PK, FK에 bigint 적용 과정
데이터베이스에서 PK와 FK를 보통 int로 정의했다. int는 42억 9천만 까지의 숫자를 가지며, 그 이상의 숫자는 적용할 수 없다. 보통 서비스에 사용되는 PK값이 int의 한계 숫자를 넘기기 어렵지만, 만약에 넘기게 되는 서비스를 하게 된다면 int에서 bigint로 타입을 변경하는데 감당하기 어려운 일을 맡게 될 것 같다는 생각을 하게 되었다. 

그렇기 때문에 현재 서비스에서 사용되는 모든 PK, FK의 타입을 bigint로 변경을 하였다. 

당연하게도 에러가 발생했는데 희안하게 post는 되는데 update가 되지 않는다. 나의 경우 TypeORM을 사용한다. 그렇기 때문에 save 메서드를 사용하는데, 이 메서드는 기본적으로 post역할을 하지만, 입력된 값 중 key가 데이터 베이스에 존재하면 update를 시켜주는 역할을 동시에 하는 메서드이다.

bigint로 변경하고 나서 update가 먹히지 않고 Duplicate Key라는 에러가 나타나서 구글링을 해보니, MySQL의 드라이버는 디폴트로 Bigint를 string처리 해버린다고 한다.

[https://github.com/typeorm/typeorm/issues/7833](https://github.com/typeorm/typeorm/issues/7833)

이를 위해 TypeORM에서 connectionOptions로 bigNumberStrings를 false로 변경하면 number로 인식하게 된다.