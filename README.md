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

<br>
<br>
<br>
<br>

### 220512

<br>
Hover, Tap, Drag를 사용했다.
<br>
<br>
Hover은 whileHover를 사용하여 호버 제스처가 인식되는 동안 애니메이션이 할 속성을 입력한다.
<br>
<br>
Tap은 컴포넌트를 누르고 있는 동안 애니메이션이 할 속성을 입력한다.
<br>
<br>
마지막으로 Drag는 요소의 끌기를 활성화 한다. whileDrag를 사용하여 드래그 제스처가 인식되는 동안 애니메이션할 속성 을 입력한다.
<br>
<br>

```
...

// Variants를 만들어 사용한다.
const boxVariants = {
  hover: { scale: 1.5, rotateZ: 90 },
  click: { scale: 1, borderRadius: "100px" },
  drag: { backgroundColor: "rgb(46, 204, 113)", transition: { duration: 10 } },
};

function App() {
  return (
    <Wrapper>
      <Box
        drag
        variants={boxVariants}
        whileHover="hover"
        whileDrag="drag"
        whileTap="click"
      />
    </Wrapper>
  );
}

```

<br>
<br>
다음으론 Drag에 대해 깊게 들어가는데 DragConstraints와 DragSnapToOrigin, DragElastic이다.
<br>
DragConstraints는 허용된 드래그 기능에 영역을 적용한다.
<br>
두 가지 방법으로 사용이 가능하며 하나는 픽셀을 이용하는 것과 하나는 ref를 이용하는 것이다.

```
// 픽셀 이용
< motion.div drag="x" dragConstraints={{ left: 0, right: 300 }}/ >


function App() {
  // ref이용
  const biggerBoxRef = useRef<HTMLDivElement>(null);
  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag
          dragConstraints={biggerBoxRef}
          variants={boxVariants}
        />
      </BiggerBox>
    </Wrapper>
  );
}
```

<br>
<br>
DragSnapToOrigin은 드래그한 요소를 원점으로 애니메이션하는 기능이고
<br>
DragElastic은 잡아당기는 힘을 뜻한다. 숫자 크기에 따라 잡아당기는 힘의 크기가 달라진다.
기본 값은 0.5이다

```
function App() {
  const biggerBoxRef = useRef<HTMLDivElement>(null);
  return (
    <Wrapper>
      <BiggerBox ref={biggerBoxRef}>
        <Box
          drag
          dragSnapToOrigin
          dragElastic={0.5}
          dragConstraints={biggerBoxRef}
          variants={boxVariants}
          whileHover="hover"
          whileTap="click"
        />
      </BiggerBox>
    </Wrapper>
  );
}
```

<br>
<br>
다음으론 MotionValue를 배웠다.
<br>
MotionValue는 애니메이션 값의 상태와 속도를 추적한다. 모든 모션 컴포넌트는 내부적으로 MotionValue를 사용한다.
<br>
<br>
재미있게도 여기서 사용하는 모션 라이브러리에선 set, get, onChange등의 함수를 제공하는데 이는 React State가 아니기 때문에 리렌더링이 일어나지 않는다.
<br>
<br>

```
// 아래와 같이 useEffect를 사용하여 drag할 때 x 값을 가져올 수 있다.
function App() {
  const x = useMotionValue(0);
  useEffect(()=>{
    x.onChange(()=> console.log(x.get()));
  }, [x]);
  return (
    <Wrapper>
      <Box style={{ x }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```

<br>
위와 같이 MotionValue 값을 가져와 useTransform을 사용해 연결할 수 있다.
<br>
<br>
useTransform은 한 값 범위에서 다른 값 범위로 매핑하여 다른 MotionValue의 output을 변환하는 MotionValue를 만든다.
<br>

```
function App() {
  const x = useMotionValue(0);

  // MotionValue가 -800일 때 scale은 2, 800일 때 0.1로 변하게 한다.
  const scale = useTransform(x, [-800, 0, 800], [2, 1, 0.1]);
  return (
    <Wrapper>
      <Box style={{ x, scale }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}
```

<br>
<br>
마지막으로 useViewportScroll이 있는데 스크롤 될 때 업데이트 되는 MotionValue를 리턴해준다.
<br>
이걸 사용해서 스크롤 이동 시에 애니메이션 효과를 줄 수가 있다.
<br>
일반적인 Scroll은 px을 ScrollProgress는 %를 나타내어 사용한다.

```
function App() {
  const x = useMotionValue(0);
  const rotateZ = useTransform(x, [-800, 800], [-360, 360]);
  const gradient = useTransform(
    x,
    [-800, 0, 800],
    [
      "linear-gradient(135deg, rgb(0, 210, 238), rgb(0, 83, 238))",
      "linear-gradient(135deg, rgb(238, 0, 153), rgb(221, 0, 238))",
      "linear-gradient(135deg, rgb(0, 238, 155), rgb(238, 178, 0))",
    ]
  );
  const { scrollYProgress } = useViewportScroll();
  // Y축 progress가 0일 때 scale은 1, 1일 때 scale은 5로 변화를 준다.
  const scale = useTransform(scrollYProgress, [0, 1], [1, 5]);
  return (
    <Wrapper style={{ background: gradient }}>
      <Box style={{ x, rotateZ, scale }} drag="x" dragSnapToOrigin />
    </Wrapper>
  );
}

```

<br>
<br>
<br>
<br>

### 220513

<br>
오늘 배운 내용은 SVG 애니메이션이다.
<br>
svg 엘리멘트에 애니메이션 효과를 주는것이다.
<br>
강의에선 FontAwesome에서 Airbnb로고를 다운 받아 사용했다.
<br>
svg도 variants를 사용할 수 있고 start(initial)와 end(animate)를 사용하여 효과를 준다.
<br>
transition에 duration 시간을 설정해 애니메이션 효과를 주고
<br>
svg에 stroke(선)의 색상 및 두께도 설정이 가능하다.

```
const Svg = styled.svg`
  width: 300px;
  height: 300px;
  path {
    stroke: white;
    stroke-width: 2;
  }
`;

const svg = {
  start: { pathLength: 0, fill: "rgba(255, 255, 255, 0)" },
  end: {
    fill: "rgba(255, 255, 255, 1)",
    pathLength: 1,
  },
};

function App() {
  return (
    <Wrapper>
      <Svg
        focusable="false"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 448 512"
      >
        <motion.path
          variants={svg}
          initial="start"
          animate="end"
          transition={{
            default: { duration: 5 },
            fill: { duration: 1, delay: 3 },
          }}
          d= ""
        ></motion.path>
      </Svg>
    </Wrapper>
  );
}

```

<br>
<br>

다음으론 AnimatePresence 이다.
<br>
AnimatePresence를 사용하면 React 트리에서 컴포넌트가 제거될 때 제거되는 컴포넌트에 애니메이션 효과를 줄 수 있다.
<br>
또한 exit라는 속성도 있는데 이는 엘리멘트가 사라질 때의 애니메이션을 동작시켜준다.
<br>

```
const boxVariants = {
  initial: {
    opacity: 0,
    scale: 0,
  },
  visible: {
    opacity: 1,
    scale: 1,
    rotateZ: 360,
  },
  leaving: {
    opacity: 0,
    scale: 0,
    y: 50,
  },
};

function App() {
  // useState와 button 태그의 onClick을 만들었다.
  const [showing, setShowing] = useState(false);
  const toggleShowing = () => setShowing((prev) => !prev);
  return (
    <Wrapper>
    // 버튼을 클릭하면 Box가 애니메이션과 함께 나오고 사라진다.
      <button onClick={toggleShowing}>Click</button>
      <AnimatePresence>
        {showing ? (
          <Box
            variants={boxVariants}
            initial="initial"
            animate="visible"
            exit="leaving"
          />
        ) : null}
      </AnimatePresence>
    </Wrapper>
  );
}
```

<br>
<br>

이후 위의 AnimatePresence를 이용하여 슬라이드를 만들었다.
<br>

```
const box = {
  entry: (isBack: boolean) => {
    return {
      x: isBack ? -500 : 500,
      opacity: 0,
      scale: 0,
    };
  },
  center: (isBack: boolean) => {
    return {
      x: 0,
      opacity: 1,
      scale: 1,
      transition: {
        duration: 0.5,
      },
    };
  },
  exits: (isBack: boolean) => {
    return {
      x: isBack ? 500 : -500,
      opacity: 0,
      scale: 0,
      transition: {
        duration: 0.5,
      },
    };
  },
};

function App() {
  // visible은 Box를 생성하기 위해 사용된다.
  const [visible, setVisible] = useState(1);

  // back은 next와 prev 버튼의 분리를 위해 사용된다.
  const [back, setBack] = useState(false);

  // next버튼은 슬라이드가 끝에서 버튼을 누르면 다시 처음으로 가도록 설정함
  const nextPlease = () => {
    setBack(false);
    setVisible((prev) => (prev === 10 ? 1 : prev + 1));
  };

  // prev버튼은 슬라이드가 처음에서 버튼을 누르면 다시 끝으로 가도록 설정함
  const prevPlease = () => {
    setBack(true);
    setVisible((prev) => (prev === 1 ? 10 : prev - 1));
  };
  return (
    <Wrapper>
      <AnimatePresence custom={back}>
        <Box
        // custom 속성을 활용
          custom={back}
          variants={box}
          initial="entry"
          animate="center"
          exit="exits"
          key={visible}
        >
          {visible}
        </Box>
      </AnimatePresence>
      <button onClick={prevPlease}>prev</button>
      <button onClick={nextPlease}>next</button>
    </Wrapper>
  );
}
```

또한 exitBeforeEnter이라는 속성도 있는데 이걸 사용하면 AnimatePresence의 애니메이션이 하나씩 렌더링된다.

<br>
<br>
<br>
<br>

### 220516

<br>
애니메이션 섹션의 마지막으로 클릭 시 확대 혹은 단독으로 중앙 배치되는 애니메이션을 배웠다.
<br>

```
function App() {
  const [id, setId] = useState<null | string>(null);
  return (
    <Wrapper>
      <Grid>

        // 4개의 박스를 만들기 위해 배열로 4개의 각기 다른 박스를 만들었다.
        {[1, 2, 3, 4].map((n) => (
          <Box onClick={() => setId(n + "")} key={n} layoutId={n + ""} />
        ))}
      </Grid>
      <AnimatePresence>
        {id ? (
          <Overlay

            // 클릭 시 setId를 null로 만들어 중앙 배치된 Box를 다시 원상복구 시켜준다.
            onClick={() => setId(null)}

            // 중앙 배치될 때 주변 배경이 어두워지고 축소 될 땐 다시 밝아지도록 했다.
            initial={{ backgroundColor: "rgba(0, 0, 0, 0)" }}
            animate={{ backgroundColor: "rgba(0, 0, 0, 0.5)" }}
            exit={{ backgroundColor: "rgba(0, 0, 0, 0)" }}
          >
          // 선택한 Box가 width 400, heigt 200의 사이즈로 중앙에 나오게 된다.
            <Box layoutId={id} style={{ width: 400, height: 200 }} />
          </Overlay>
        ) : null}
      </AnimatePresence>
    </Wrapper>
  );
}
```

<br>
<br>

Animation 파트가 마무리 되었는데 개인적으로<br>
Farmer Motion이라는 라이버러리는 자주 사용할 거 같다.
