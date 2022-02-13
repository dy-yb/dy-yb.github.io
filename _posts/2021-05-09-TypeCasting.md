---
title: Type Casting
date: 2021-05-09 20:08:00
tags: apple_docs swift
---

- [Apple Swift Documentation](https://docs.swift.org/swift-book/LanguageGuide/TypeCasting.html) 번역

### When?

- 인스턴스의 타입을 체크 할 때
- 인스턴스의 타입을 슈퍼/서브(부모/자식) 클래스의 타입처럼 사용할 때

### How?

- 연산자: `is`, `as`

- **타입캐스팅을 위한 클래스 계층구조 정의**
    - 클래스 및 서브클래스의 계층구조와 함께 타입 캐스팅을 통해 특정 클래스 인스턴스의 타입을 확인하고, 같은 계층구조 내에서 인스턴스를 다른 클래스로 캐스팅 할 수 있습니다.
    - 예제
        ```swift
        class MediaItem {
        	var name: String
        	init(name: String) {
        		self.name = name
        	}
        }

        class Movie: MediaItem {
        	var director: String
        	init(name: String, director: String) {
        		self.director = director
        		super.init(name: name)
        	}
        }

        class Song: MediaItem {
        	var artist: String
        	init(name: String, artist: String) {
        		self.artist = artist
        		super.init(name: name)
        	}
        }

        let library = [
        	Movie(name: "Casablanca", director: "Michael Curtiz"),
        	Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
            Movie(name: "Citizen Kane", director: "Orson Welles"),
            Song(name: "The One And Only", artist: "Chesney Hawkes"),
            Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
        ]
        // library의 타입은 타입 추론으로 인해 [MediaItem]이 됩니다.
        ```

- **타입 체크하기(is)**
    - `is` 연산자를 사용하면 인스턴스가 특정 하위 클래스 타입인지 체크할 수 있습니다. 타입체크 연산자는 인스턴스가 서브클래스 타입이면 `true`를, 아니면 `false`를 반환합니다.
    - 예제
        ```swift
        var movieCount = 0
        var songCount = 0

        for item in library {
            if item is Movie {
                movieCount += 1
            } else if item is Song {
                songCount += 1
            }
        }

        print("Media library contains \(movieCount) movies and \(songCount) songs")
        // 결과: "Media library contains 2 movies and 3 songs"
        ```

- **Downcasting**
    - 특정 클래스 타입의 상수 혹은 변수가 서브클래스의 인스턴스를 참조하는 경우가 있는데, 이 때 `as?` 혹은 `as!` 연산자를 사용하여 서브클래스 타입으로 다운캐스트 할 수 있습니다. 다운캐스팅은 실패할 수 있기때문에, 타입캐스트 연산자는 두 가지 형태로 사용합니다. 조건부 형식의 `as?`는 다운캐스트 시도시 optional 값을 반환하고, 강제 형식 `as!`는 다운 캐스트를 시도 후 강제 언래핑 합니다.
    - 조건부 형식의 연산자 `as?`는 다운캐스트가 성공할지 확실하지 않을 때 사용합니다. 이 형식은 항상 옵셔널 값을 반환하고 만약 다운캐스트가 실패하면 `nil` 이 반환됩니다. 이 형식은 다운캐스트가 성공했는지 확인하는 용도로도 사용 가능합니다.
    - 강제 형식의 연산자 `as!`는 다운캐스트 성공에 확신이 있는 경우에만 사용합니다. 이 연산자 형식은 잘못된 클래스 타입으로 다운캐스트를 시도한 경우 런타임 에러의 원인 되기도 합니다.
    - 아래 예제는 `library`에 각 `MediaItem`을 반복하고, 각 아이템에 알맞는 설명을 프린트 합니다. 이것을 위해서는 `MediaItem`이 아닌 `Movie` 혹은 `Song`으로 접근해야 합니다. `Movie` 혹은 `Song`의 프로퍼티에 접근하여 각각의 알맞은 설명을 출력하기 위해서 입니다. 각 `item`에 사용될 클래스가 무엇인지 미리 알 수 없으므로 반복문이 실행될때 조건부 형식 연산자인 `as?`를 사용하여 체크하도록 합니다.

    ```swift
    for item in library {
        if let movie = item as? Movie {
            print("Movie: \(movie.name), dir. \(movie.director)")
        } else if let song = item as? Song {
            print("Song: \(song.name), by \(song.artist)")
        }
    }

    // Movie: Casablanca, dir. Michael Curtiz
    // Song: Blue Suede Shoes, by Elvis Presley
    // Movie: Citizen Kane, dir. Orson Welles
    // Song: The One And Only, by Chesney Hawkes
    // Song: Never Gonna Give You Up, by Rick Astley
    ```

    - 위 예제는 `item`을 `Movie`로 다운캐스트 하는 것으로 시작합니다. `item`은 `MediaItem` 인스턴스이기 떄문에 `Movie`도 될 수 있고, `Song`도 될 수 있고, 또는 그냥 `MediaItem`를 일 수도 있습니다. 이 불확실성 때문에 `as?`는 서브 클래스 타입으로 다운캐스트를 시도할 때 옵셔널 값을 반환합니다. `item as? Movie`의 결과 값은 `Movie?`(옵셔널 Movie) 입니다.
    - `library` 배열에 `Song` 인스턴스로 적용할 때 `Movie`로 다운캐스팅을 하면 실패합니다. 이 것을 해결하기 위해 위 예제는 `Movie?`가 실제로 값을 포함하는지(다운캐스트가 성공했는지) 확인하기 위해 옵셔널 바인딩을 사용합니다. 옵셔널 바인딩은 `if let movie = item as? Movie` 와 같이 작성 할 수 있으며 다음과 같이 이해하면 됩니다. "item이 Movie로 접근을 시도합니다. 만약 성공하면, 반환된 Movie?의 값을 새 임시 상수인 movie에 설정하세요."
    - 만약 다운캐스팅이 성공하면, `movie`의 프로퍼티는 `director`의 이름을 포함한 `Movie` 인스턴스에 대한 설명이 출력되는데 사용될 것입니다. 같은 원리로 `Song` 인스턴스도 체크할 수 있고 `Song` 인스턴스는 `library` 내에서 찾을 때마다 알맞은 설명을(`artist` 이름 포함) 출력할 수 있습니다.
    - 캐스팅은 인스턴스를 수정하거나 값을 변경하지 않습니다. 기본 인스턴스는 동일하게 유지되며 캐스트 된 타입의 인스턴스로 처리, 접근 됩니다.
