# React Master Class

### 노마드코더 강의 내용 정리 및 복습을 위한 README 입니다.

<br>
<br>
<br>
<br>

### 220511

<br>
Animaion에 대한 강의이며, Framer Motion이라는 모션 라이브러리를 사용한다.
<br>
<br>
https://www.framer.com/motion 👈 해당 사이트 주소
<br>
https://github.com/framer/motion 👈 해당 깃허브 주소
<br>
<br>
아래 명령어로 설치를 해준다.

```
$ npm i framer-motion
```

<br>
<br>
설치 이후 아래와 같은 에러가 발생할 수 있다.

```
Failed to compile.

./node_modules/framer-motion/dist/es/components/AnimatePresence/index.mjs
Can't import the named export 'Children' from non EcmaScript module (only default export is available)
```

이는 react-scripts 버전이 4.x.x이라서 나타나는 것인데
<br>
이를 해결하기 위해선 설정사항을 덮어쓰기 해야한다.
<br>
<br>
이때 사용하는 것이 CRACO이다.
<br>
<br>
https://github.com/gsoft-inc/craco 👈 해당 깃허브 주소
<br>
<br>

먼저 아래 명령어로 설치를 해준다.

```
$ npm install @craco/craco --save

```

이후 craco.config.js 파일을 만들어 아래 코드를 붙여넣는다.

```
module.exports = {
  webpack: {
    configure: {
      module: {
        rules: [
          {
            type: "javascript/auto",
            test: /\.mjs$/,
            include: /node_modules/,
          },
        ],
      },
    },
  },
};

```

마지막으로 package.json파일에 script를 react-scripts 를 craco로 바꾸어주면 된다.

```
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```

<br>
<br>
설치는 끝났고 본격적으로 Animation을 들어간다.
<br>
기본적인 motion의 사용법은 tag앞에 motion을 붙이는 것이다.
<br>
styled를 사용하여 만들 수도 있는데 styled(motion.div)`` 형태로 생성이 가능하다.

```
const Box = styled.(motion.div)`
    width: 100px;
    heigh: 100px;
    background-color: white;
`


function App() {
  return (
    <Wrapper>
      // 기본적인 예제
      <motion.div></motion.div>
      // styled 사용 예제
      <Box/>
    </Wrapper>
  );
}
```

<br>
motion의 props들을 살펴볼텐데 기본적으로
<br>
animate, trasition, initial가 사용된다.

```
function App() {
  return (
    <Wrapper>
      <Box
      transition ={
          { type: "spring"}
      }
      initial={{ scale: 0 }}
      animate={{ scale: 1 }} />
    </Wrapper>
  );
}
```

하지만 위와 같이 사용하면 코드가 길어지고 지저분해 보일 수 있다.
<br>
그래서 사용하는 것이 Variants인데 위의 props들을 넣어 사용하는 변수이다.
<br>

```
const boxVariants = {
  start: {
    scale: 0,
  },
  end: {
    scale: 1,
  },
};

function App() {
  return (
    <Wrapper>
      // initial는 (초기)상태 뜻하고 animate은 동작을 뜻한다.
      <Box variants={boxVariants} initial="start" animate="end" />
    </Wrapper>
  );
}

```

마지막으로 부모 요소가 자식 요소에게 효과를 넣어 줄 수 있는데
<br>
대표적으로 delayChildren과 staggerChildren이 있다.
<br>
delayChildren은 자식 요소 모두에게 delay를 주는 것이며,
<br>
staggerChildern은 자식 요소를 delay 효과를 준다.

```
const boxVariants = {
  start: {
    opacity: 0,
    scale: 0.5,
  },
  end: {
    opacity: 1,
    scale: 1,
    transition: {
      type: "spring",
      duration: 1.2,
      bounce: 0.5,
      delayChildren: 0.5,
      staggerChildren: 0.2,
    },
  },
};

const circleVariants = {
  start: {
    opacity: 0,
    y: -10,
  },
  end: {
    opacity: 1,
    y: 0,
  },
};

function App() {
  return (
    <Wrapper>
      <Box variants={boxVariants} initial="start" animate="end">
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
        <Circle variants={circleVariants} />
      </Box>
    </Wrapper>
  );
}
```
