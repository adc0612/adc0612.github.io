---
layout: post
comments: true
title: "프론트엔드 테스트 Jest"
description: "프론트엔드 테스트 Jest"
subject: vue
categories: [ vue ]
tags: [ vue, frontend test, jest]
---

<hr>

## 프론트엔드 테스트가 필요한 이유

예로 로그인페이지만 하더라도 일일히 화면에서 치고 확인하며 테스트를 해야한다.  
그 수고를 덜기위해 테스팅을 활용하는 것이다.

{% highlight markdown %}
* id input 박스에 이메일을 입력했을 때 이메일이 맞는지 확인
* id, pw가 맞는 경우에 로그인 처리되는지 확인 및 다음 페이지로 이동 확인
{% endhighlight %}

<hr>

## Jest

Jest는 Facebook에서 만든 Javascript Testing Framework이기.  
사용하기도 쉽고 많은 기능을 지원을 하는 대중적인 Testing Framework이기 때문이다.

[Jest 2019기준 점유율](https://2019.stateofjs.com/testing/){: target="_blank"}

[Jest 공식문서](https://jestjs.io/en/){: target="_blank"}

<hr>

## Jest 설치 및 설정

Jest 설치는 npm으로 설치 가능하다.

```bash
npm install --save-dev jest
```

지금 프로젝트는 vue-cli로 프로젝트 만들 때 Jest를 포함해서 설치했기 때문에 별도에 설치가 필요없다.

보통은 test파일들을 `tests/unit`폴더안에 넣는다.   
`src/jest.config.js`jest config파일을 수정한다.  
소스폴더 안에있는 `.spec.(js|jsx|ts|tsx)`로 끝나는 파일과 `__tests__`폴더 안에 있는 파일들을 테스팅 코드로 인식 할 수 있게 jest환경설정을 한다. 

`src/jest.config.js`
```javascript
module.exports = {
  preset: '@vue/cli-plugin-unit-jest',
  testMatch: [
    '<rootDir>/src/**/*.spec.(js|jsx|ts|tsx)|**/__tests__/*.(js|jsx|ts|tsx)',
  ],
};
```

[Jest 문법 공식 문서](https://jestjs.io/docs/en/api){: target="_brank"}

jest 문법 (api)은 eslint가 에러로 잡아내므로 `.eslintrc.js`에 jest문법을 추가해서 에러로 못 잡게 한다.

![eslint jest에러](/assets/img/vue/vue-jest1.png "eslint jest에러")

`.eslintrc.js`
```javascript
  env: {
    node: true,
    // jest 문법에 빨간줄쳐지지 않게한다.
    jest: true,
  },
```

<hr>

## Jest 명령어 설정

`src/pakage.json`에서 jest를 실행하는 명령어를 편집한다.  
VueCli는 jest실행 명령어가 기본적으로 test:unit이지만 학습을 위해 test로 바꾸어 두었다.  
test코드 파일이 변화되면 다시 자동으로 실행하는 --watchAll 옵션도 추가한다.  

`src/pakage.json`
```javascript
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "test": "vue-cli-service test:unit --watchAll",
    "lint": "vue-cli-service lint"
  },
```

<hr>

## Jest 테스트 실행

`npm t`로 테스트를 실행한다.

```bash
npm t
```

<hr>

## ID가 이메일 형식이 아니면 경고 메시지가 출력 테스트

`LoginForm.vue`에서 ID가 이메일 형식이 아니면 경고 메시지가 잘 뜨는지 테스트케이스를 만들었다.  

`LoginForm.spec.js`
```javascript
// shallowMount는 vue에서 제공하는 test utils다.
import { shallowMount } from '@vue/test-utils';
import LoginForm from './LoginForm.vue';

// describe(): 연관된 테스트 케이스를 그룹화하는 API
describe('LoginForm.vue', () => {
  // test(): 하나의 테스트 케이스를 검증하는 API
  test('ID가 이메일 형식이 아니면 경고 메시지가 출력 된다.', () => {
    //  wrapper에 shallowMount로 LoginForm 컴포넌트를 mount한다.
    // data안에 username을 입력하면 아래 값으로 username이 binding되면서 테스트를 진행하게 된다.
    const wrapper = shallowMount(LoginForm, {
      data() {
        return {
          username: 'test@',
        };
      },
    });
    // warning class를 가진 input박스를 find()함수로 찾는다.
    const warningText = wrapper.find('.warning');
    // toBeTruthy()함수는 앞에 있는 값이 true인지 확인이다.
    expect(warningText.exists()).toBeTruthy();
  });
});
```

username으로 `이메일 형식이 아닌 텍스트`를 입력하고 테스트를 한다.  
경고메세지가 뜨면 테스트 통과가 된다.  

![jest test 성공 결과](/assets/img/vue/vue-jest2.png "jest test 성공 결과")

만약 username으로 `이메일 형식인 텍스트`를 입력하고 테스트를 하면,  
경고메세지가 뜨지 않으므로 테스트 실패가 된다.  
(이 케이스의 테스트 실패는 기능은 잘 작동 된다는 것이다.)

![jest test 실패 결과](/assets/img/vue/vue-jest3.png "jest test 실패 결과")

<hr>

## ID와 PW값이 없으면 로그인버튼이 잘 비활성화 되는지 확인

`LoginForm.vue`에서 ID와 PW값이 없으면 로그인 버튼이 잘 비활성화 되는지 확인하는 테스트케이스를 만들었다.  

`LoginForm.spec.js`
```javascript
  //   ID와 PW값이 없으면 로그인버튼이 잘 비활성화 되는지 확인
  test('ID와 PW가 입력되지 않으면 로그인 버튼이 비활성화 된다.', () => {
    const wrapper = shallowMount(LoginForm, {
      data() {
        return {
          username: '',
          password: '',
        };
      },
    });
    const button = wrapper.find('button');
    expect(button.element.disabled).toBeTruthy();
  });
```

<hr>



