## 5장 - 형식 맞추기

**프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야 한다.**

- 코드 형식을 맞추기 위한 간단한 규칙을 정하고 그 규칙을 착실히 따라야 한다.
- 팀으로 일한다면 팀이 합의해 규칙을 정하고 모두가 그 규칙을 따라야 한다.
- 필요하다면 규칙을 자동으로 적용하는 도구를 활용한다.

## 형식을 맞추는 목적

**원래 코드는 사라질지라도 개발자의 스타일과 규율은 사라지지 않는다.**

- 오랜 시간이 지나 원래 코드의 흔적을 더 이상 찾아보기 어려울 정도로 코드가 바뀌어도 맨 처음 잡아놓은 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 미친다.

## 적절한 행 길이를 유지하라

**일반적으로 큰 파일보다 작은 파일이 이해하기 쉽다**

- 소스 파일도 신문 기사와 비슷하게 작성한다.

## 개념은 빈행으로 분리하라

생각 사이는 빈 행을 넣어 분리해야 마땅하다.

생각이 필요한 변수라고 생각되면 한칸을 띄어서 작성하는 것도 좋다.

```javascript
  ...
  constructor(isIE) {
    this._____ = new _____(this);
    this._____ = new _____(this);
    this._____ = new _____(this);
    this._____ = new _____(this);
    this._____ = new _____(this);

    this.isIE = isIE;
    ...
  }
  ...
```

코드 가독성에도 많은 차이를 줄 수 있다.

```javascript
const renderAlarmList = (): void => {
  const alarmInnerElement = $('.alarm-inner') as HTMLDivElement;

  if (!alarmInnerElement) {
    return;
  }
  alarmInnerElement.innerHTML = '';
  alarmInnerElement.insertAdjacentHTML('beforeend', alarmListWrapper());
};

const renderAlarmPage = (navigation: string): void => {
  const appId = $('#app') as HTMLDivElement;

  appId.innerHTML = '';
  appId.insertAdjacentHTML('beforeend', alarmWrapper(navigation, alarmListWrapper()));
};

export { renderAlarmPage, alarmInputWrapper, renderAlarmList };
```

```javascript
const renderAlarmList = (): void => {
  const alarmInnerElement = $('.alarm-inner') as HTMLDivElement;
  if (!alarmInnerElement) {
    return;
  }
  alarmInnerElement.innerHTML = '';
  alarmInnerElement.insertAdjacentHTML('beforeend', alarmListWrapper());
};
const renderAlarmPage = (navigation: string): void => {
  const appId = $('#app') as HTMLDivElement;
  appId.innerHTML = '';
  appId.insertAdjacentHTML('beforeend', alarmWrapper(navigation, alarmListWrapper()));
};
export { renderAlarmPage, alarmInputWrapper, renderAlarmList };
```

## 세로 밀집도

주석이 두 함수 사이의 거리를 떨어뜨려 놓았다. 함수의 경우도 불필요한 주석으로 가독성이 낮아지는데, 인스턴스 변수나, 다른 연관있는 함수의 경우 더더욱 가독성에 심각한 영향을 줄 수 있다.

```c
/* ************************************************************************** */
/*                                                                            */
/*                                                        :::      ::::::::   */
/*   prompt.c                                           :+:      :+:    :+:   */
/*                                                    +:+ +:+         +:+     */
/*   By: holee <holee@student.42.fr>                +#+  +:+       +#+        */
/*                                                +#+#+#+#+#+   +#+           */
/*   Created: 2020/09/15 22:02:35 by holee             #+#    #+#             */
/*   Updated: 2021/01/03 16:48:32 by holee            ###   ########.fr       */
/*                                                                            */
/* ************************************************************************** */

#include "minishell.h"

/*
**		prompt
**
**		Printed in minishell.
**
**		@param	char*	username	User name stored in environment variable
*/

void		ft_prompt(char *username)
{
	char	*cwd;

	cwd = getcwd(0, 1024);
	ft_putstr_fd("\033[32m\033[01m", 1);
	ft_putstr_fd(username, 1);
	ft_putstr_fd("@minishell\033[0m", 1);
	ft_putstr_fd("\033[36m\033[01m ", 1);
	ft_putstr_fd(ft_strrchr(cwd, '/') + 1, 1);
	ft_putstr_fd("\033[35m % \033[0m", 1);
	free(cwd);
	return ;
}

/*
**		ft_cmd_not_found
**
**		Print to minishell if command not found.
**
**		@param	char*	command 	print command string
*/

void		ft_cmd_not_found(char *command)
{
	ft_putstr_fd("minishell: \033[31mcommand not found: ", 2);
	ft_putstr_fd(command, 2);
	ft_putstr_fd("\n\033[0m", 2);
	return ;
}
```

````c
#include "minishell.h"

void		ft_prompt(char *username)
{
	char	*cwd;

	cwd = getcwd(0, 1024);
	ft_putstr_fd("\033[32m\033[01m", 1);
	ft_putstr_fd(username, 1);
	ft_putstr_fd("@minishell\033[0m", 1);
	ft_putstr_fd("\033[36m\033[01m ", 1);
	ft_putstr_fd(ft_strrchr(cwd, '/') + 1, 1);
	ft_putstr_fd("\033[35m % \033[0m", 1);
	free(cwd);
	return ;
}

void		ft_cmd_not_found(char *command)
{
	ft_putstr_fd("minishell: \033[31mcommand not found: ", 2);
	ft_putstr_fd(command, 2);
	ft_putstr_fd("\n\033[0m", 2);
	return ;
}
````

### 수직 거리

- 서로 밀집한 개념은 세로로 가까이 둬야 한다.
- 타당한 근거가 없다면 서로 밀집한 개념은 한 파일에 속해야 마땅하다.
- **변수**는 사용하는 위치에 최대한 가까이 선언한다. 단, 함수가 짧을 경우 각 함수 맨 처음에 선언한다.
- **인스턴스 변수**는 잘 알려진 위치에 모은다.

```typescript
class Rect {
  width : number;
  height: number;
  static count: number;

  constructor(width, height) {
    this.width = width;
    this.height = height;
    Rect.count++;
  }

	static getAreaCount: number;

  getArea(): number {
    Rect.getAreaCount++;
    return this.width * this.height;
  }
}
```

```typescript
class Rect {
  width : number;
  height: number;
  static count: number;
  static getAreaCount: number;

  constructor(width, height) {
    this.width = width;
    this.height = height;
    Rect.count++;
  }
  
  getArea(): number {
    Rect.getAreaCount++;
    return this.width * this.height;
  }
}
```

- 한 함수각 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다. 또한, 가능하다면 호출하는 함수를 호출되는 함수보다 먼저 배치한다. 
  - 그래야 자연스럽게 위에서 아래로 보면서 다음에 호출된 합수를 예측하며 볼 수 있다.

```javascript
const appIconWrapper = (appName: string, appRoute: string): string => {
  return `<div class="dragzone">
            <button class="app-icon draggable" draggable="true" data-route="${appRoute}">
            ${appName}
            </button>
          </div>`;
};

const homeWrapper = (navigation: string, apps: string): string => {
  return `<section class="home">
            ${navigation}
            <nav class="home-inner">
              ${apps}
            </nav>
          </section>`;
};

// 코드 수정시 LocalStorage가 존재해서, Update 사항이 반영되지 않는 위험이 있음.
const getData = (appDatas: IappData[]): IappData[] => {
  const localAppDatas = model.getLocalStorageAppData('appDatas');
  if (localAppDatas === null) {
    model.setLocalStorageAppData('appDatas', appDatas);
    return appDatas;
  }
  return localAppDatas;
};

const renderHomePage = (navigation: string): void => {
  const appDatas: IappData[] = getData(appInitDatas);
  const appId = $('#app') as HTMLDivElement;

  appId.innerHTML = '';
  appId.insertAdjacentHTML(
    'beforeend',
    homeWrapper(
      navigation,
      appDatas
        .sort((a, b) => a.order - b.order)
        .map((appData) => {
          return appIconWrapper(appData.name, appData.route);
        })
        .join(''),
    ),
  );
};
```

```javascript
const renderHomePage = (navigation: string): void => {
  const appDatas: IappData[] = getData(appInitDatas);
  const appId = $('#app') as HTMLDivElement;

  appId.innerHTML = '';
  appId.insertAdjacentHTML(
    'beforeend',
    homeWrapper(
      navigation,
      appDatas
        .sort((a, b) => a.order - b.order)
        .map((appData) => {
          return appIconWrapper(appData.name, appData.route);
        })
        .join(''),
    ),
  );
};

// 코드 수정시 LocalStorage가 존재해서, Update 사항이 반영되지 않는 위험이 있음.
const getData = (appDatas: IappData[]): IappData[] => {
  const localAppDatas = model.getLocalStorageAppData('appDatas');
  if (localAppDatas === null) {
    model.setLocalStorageAppData('appDatas', appDatas);
    return appDatas;
  }
  return localAppDatas;
};

const homeWrapper = (navigation: string, apps: string): string => {
  return `<section class="home">
            ${navigation}
            <nav class="home-inner">
              ${apps}
            </nav>
          </section>`;
};

const appIconWrapper = (appName: string, appRoute: string): string => {
  return `<div class="dragzone">
            <button class="app-icon draggable" draggable="true" data-route="${appRoute}">
            ${appName}
            </button>
          </div>`;
};
```

하지만, 이 코드를 처음 고쳐보았을때 크게 와닿지는 않았다. 오히려 그동안 쓰던 방식인 호출하는 함수를 호출되는 함수보다 늦게 작성하는것이 더 이해하기 좋아 보였다. 그 이유는 이렇게 할 경우 비교적 최소단위 즉, 짧아서 이해하기 쉬운 함수들이 위로 올라가면서 전체적인 코드의 가독성이 올라간다고 생각했기 때문이다. 

하지만.. 생각을 하고보니 함수 내에서 동등한 추상화 단계가 잘 이뤄진다면, 호출하는 함수를 호출되는 함수보다 먼저 배치하는게 더 가독성에 좋다는 사실을 깨달았다. 물론, 코드 줄이 길어질때도 마찬가지이다. 아래 코드를 보면 확실히 느낄 수 있다.

하지만, 직접 작성해보니 그동안 책대로 호출하는 함수를 호출되는 함수보다 먼저 배치하지 않는 이유가 있었다. `const initController = (): void => {}` 다음과 같이 함수를 정의할 경우 책의 방식은 사용할 수 없다.

```javascript
import { $, atoi, getCurrentDate } from '../@shared/utils';
import { renderAlarmList } from '../view/alarm-page';
import { historyRouter } from '../@shared/router';
import homePageController from './home-page';
import model from '../model';

const initController = (): void => {
  navigationController();
  historyController();
  homePageController();
};

const navigationController = () => {
  setInterval(() => {
    updateNavigationTime();
    NotifyAlarm();
  }, 1000);
};

const updateNavigationTime = () => {
  const navTimeElement = $('.nav__time') as HTMLSpanElement;
  const currentDate = getCurrentDate();
  navTimeElement.innerText = currentDate;
}

const NotifyAlarm = (currentDate: string): void => {
  ...
  });
};

export default initController;
```

## 가로 형식 맞추기

- 프록그래머는 명백하게 짧은 행을 선호한다.
- 홀러리스는 80자 제한을 제안했다. 하지만 100자나 120자까지도 나쁘지 않다.

나의 prettier 설정

```json
{
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 120
}
```

### 가로 정렬

아래와 같이 변수 선언부를 읽다보면, 타입과 변수를 갖이 매칭해서 읽기각 힘드다는 사실을 알 수 있다. 목록이 길다면 문제는 목록 길이지 정렬 부족이 아니다.

```c++
template<typename T>
class vector
{
	public:
		typedef T																	value_type;
		typedef size_t														size_type;
		typedef ptrdiff_t													difference_type;
		typedef T																	&reference;
		typedef const T														&const_reference;
		typedef T																	*pointer;
		typedef const T														*const_pointer;
		typedef vectorIterator<const T>						const_iterator;
		typedef vectorIterator<T>									iterator;
		typedef reverse_iterator<const_iterator> 	const_reverse_iterator;
		typedef reverse_iterator<iterator> 				reverse_iterator;
	private:
		typedef vector<T> _self;

		pointer						_p;
		size_type					_length;
		size_type					_capacity;
		...
}
```

```c++
template<typename T>
class vector
{
	public:
		typedef T	value_type;
		typedef size_t size_type;
		typedef ptrdiff_t	difference_type;
		typedef T &reference;
		typedef const T &const_reference;
		typedef T *pointer;
		typedef const T	*const_pointer;
		typedef vectorIterator<const T> const_iterator;
		typedef vectorIterator<T> iterator;
		typedef reverse_iterator<const_iterator> const_reverse_iterator;
		typedef reverse_iterator<iterator> reverse_iterator;
	private:
		typedef vector<T> _self;

		pointer _p;
		size_type _length;
		size_type _capacity;
		...
}
```

### 들여쓰기

들여쓰기는 중요하다.

```javascript
const navigationController = () => {setInterval(() => {
updateNavigationTime(); NotifyAlarm(); }, 1000); };
```

```javascript
const navigationController = () => {
  setInterval(() => {
    updateNavigationTime();
    NotifyAlarm();
  }, 1000);
};

```

### 가짜 범위

이 구조는 가능하면 피하되, 피하지 못할 경우 while 문 끝에 세미콜론 하나를 개행해서 붙여주자.

```javascrirpt
while(checkFileEnd())
;
```

## 팀 규칙

팀은 한가지 규칙에 합의해야 한다.

좋은 소프트웨어 시스템은 읽기 쉬운 문서로 이뤄진다는 사실을 기억하자. 스타일은 일관적이고 매끄러워야한다.

