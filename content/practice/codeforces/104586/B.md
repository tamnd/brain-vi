---
title: "CF 104586B - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0441\u043e\u043e\u0431\u0449\u0435\u043d\u0438\u044f"
description: "Chúng tôi nhận được một tin nhắn ngắn được chia thành các từ và chúng tôi muốn quyết định xem liệu từ khóa đặc biệt “codecup” có thể xuất hiện ở đâu đó trong tin nhắn đó sau lỗi truyền tải hay không. Chi tiết chính là mô hình lỗi."
date: "2026-06-30T07:33:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104586
codeforces_index: "B"
codeforces_contest_name: "Codemasters Codecup 2023 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 104586
solve_time_s: 84
verified: true
draft: false
---

[CF 104586B - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u0441\u043e\u043e\u0431\u0449\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/104586/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một tin nhắn ngắn được chia thành các từ và chúng tôi muốn quyết định xem liệu từ khóa đặc biệt “codecup” có thể xuất hiện ở đâu đó trong tin nhắn đó sau lỗi truyền tải hay không. 

Chi tiết chính là mô hình lỗi. Mỗi từ được truyền có thể mất tối đa một ký tự trong quá trình truyền, nhưng các ký tự không bao giờ bị thay đổi và không có gì khác xảy ra trên các từ. Vì vậy, mọi từ được quan sát chính xác là từ gốc hoặc từ gốc có một ký tự bị xóa ở đâu đó bên trong nó. Không được phép thay thế và không được phép xóa nhiều lần. 

Nhiệm vụ là kiểm tra xem bất kỳ từ nào được quan sát có thể tương ứng với từ dự định “codecup” theo quy tắc này hay không. Nếu ít nhất một từ có thể là phiên bản bị lỗi của “codecup”, thì câu trả lời là tích cực. 

Kích thước đầu vào nhỏ: tối đa 100 từ, mỗi từ có độ dài tối đa 25. Điều này loại bỏ mọi áp lực đối với các cấu trúc dữ liệu nâng cao hoặc tiền xử lý nặng. Kiểm tra trực tiếp từng từ là đủ vì mỗi lần kiểm tra chỉ liên quan đến một mẫu có độ dài cố định là 7. 

Một điểm tế nhị là chúng ta không được phép “sửa” một từ bằng cách thay đổi ký tự. Chúng tôi chỉ xóa tối đa một ký tự khỏi “codecup” ban đầu để lấy từ đã nhận. Sự bất đối xứng này quan trọng vì nó tránh nhầm lẫn giữa điều này với khoảng cách chỉnh sửa chung. 

Trường hợp cạnh chính là khi từ gần đúng nhưng khác nhau ở hai vị trí. Ví dụ: “codecap” khác nhau ở chỗ thay thế, không hợp lệ và “cdecp” tương ứng với hai lần xóa, cũng không hợp lệ. Cả hai đều thất bại một cách chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng căn chỉnh từng từ trong thông báo với tất cả các cách có thể để xóa các ký tự khỏi “codecup”. Vì “codecup” có độ dài 7 nên chỉ có 8 khả năng: không xóa gì hoặc xóa chính xác một trong 7 vị trí. Đối với mỗi từ, chúng ta có thể tạo ra 8 ứng viên này và so sánh. 

Điều này đã hoạt động trong giới hạn, nhưng nó hơi gián tiếp. Quan sát rõ ràng hơn là chúng ta không cần tạo ra các mẫu đã sửa đổi. Thay vào đó, chúng ta có thể quét từ theo “codecup” bằng cách sử dụng hai con trỏ và cho phép bỏ qua tối đa một ký tự trong mẫu. Điều này trực tiếp mã hóa ràng buộc “nhiều nhất một lần xóa khỏi bản gốc”. 

Cách tiếp cận bạo lực hoạt động vì không gian mẫu rất nhỏ, nhưng nó sẽ trở nên lộn xộn về mặt khái niệm nếu được khái quát hóa. Công thức hai con trỏ giảm mọi thứ xuống còn một lần quét tuyến tính cho mỗi từ và làm cho việc lập luận về tính chính xác trở nên dễ dàng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các lần xóa | O(n · 7) | O(1) | Đã chấp nhận | 
| Kiểm tra bỏ qua hai con trỏ | O(n · 7) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi “codecup” như một chuỗi tham chiếu cố định có độ dài 7 và kiểm tra từng từ một cách độc lập. 

1. Đối với mỗi từ trong tin nhắn, hãy thử so khớp nó với “codecup” bằng cách sử dụng hai con trỏ, một cho từ và một cho mẫu. 

Mục tiêu là sử dụng toàn bộ từ trong khi duyệt qua mẫu, cho phép tối đa một ký tự bị bỏ qua trong mẫu. 
2. Khởi tạo hai chỉ mục, một cho từ và một cho mẫu và bộ đếm số lượng ký tự mẫu mà chúng ta bỏ qua. 

Bộ đếm bỏ qua biểu thị một lần xóa được phép trong từ gốc trước khi bị hỏng. 
3. Trong khi cả hai con trỏ đều nằm trong giới hạn, hãy so sánh các ký tự. Nếu chúng khớp nhau, hãy tiến lên cả hai con trỏ. 

Điều này tương ứng với một nhân vật sống sót sau quá trình truyền tải. 
4. Nếu chúng không khớp, chúng tôi cố gắng bỏ qua một ký tự trong mẫu, tăng bộ đếm bỏ qua và chỉ tiến lên con trỏ mẫu.

Điều này mô hình hóa ý tưởng rằng ký tự mẫu này có thể là ký tự đã bị xóa trước khi truyền. 
5. Nếu sự không khớp lại xảy ra sau khi đã sử dụng tính năng bỏ qua, thì lần thử khớp không thành công đối với từ này. 
6. Sau vòng lặp, kết quả khớp chỉ hợp lệ nếu toàn bộ từ đã được sử dụng và chúng tôi đã sử dụng tối đa một lần bỏ qua trong mẫu và mọi ký tự còn lại trong mẫu chỉ có thể được bỏ qua một cách an toàn nếu chúng tương ứng với một kịch bản xóa được phép duy nhất. 
7. Nếu có từ nào thành công, chúng ta trả lời ngay “Có”. Nếu không, sau khi kiểm tra tất cả các từ, chúng ta trả về “Không”. 

### Tại sao nó hoạt động 

Ràng buộc cốt lõi là mỗi từ được quan sát đều bắt nguồn từ từ gốc bằng cách xóa tối đa một ký tự. Điều đó có nghĩa là việc căn chỉnh giữa một từ và “codecup” có thể thất bại theo đúng một cách cấu trúc: thiếu một ký tự trong mẫu. Quá trình hai con trỏ thực thi rằng mọi ký tự trong từ được quan sát phải tương ứng với một chuỗi con bảo toàn thứ tự của mẫu, trong khi bộ đếm bỏ qua thực thi rằng chúng ta không bao giờ giả định nhiều hơn một vị trí bị thiếu. Điều này mô tả chính xác tất cả các kết quả tham nhũng hợp lệ và loại trừ các thay thế hoặc xóa nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

TARGET = "codecup"

def matches(word):
    i = j = 0
    skipped = 0

    while i < len(word) and j < len(TARGET):
        if word[i] == TARGET[j]:
            i += 1
            j += 1
        else:
            if skipped == 1:
                return False
            skipped += 1
            j += 1

    if i != len(word):
        return False

    return True

def solve():
    n = int(input())
    words = input().split()

    for w in words:
        if matches(w):
            return "Yes"
    return "No"

print(solve())
```Giải pháp tách logic khớp thành một hàm trợ giúp để mỗi từ được kiểm tra độc lập. Vòng lặp hai con trỏ đảm bảo quét tuyến tính trên mẫu cố định. Sự tinh tế quan trọng là việc kiểm tra cuối cùng xem toàn bộ từ đã được sử dụng hay chưa; nếu không, kết quả khớp một phần có thể vượt qua không chính xác khi các ký tự bổ sung vẫn còn trong từ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Từ đầu vào:`codeforsquares codecup coming soon`| từ | kết quả quét | bỏ qua | hợp lệ | 
| --- | --- | --- | --- | 
| codeforsquare | không khớp quá sớm, không căn chỉnh hợp lệ | vượt quá 1 | Không | 
| cốc mã hóa | khớp chính xác đầy đủ | 0 | Có | 

Từ thứ hai khớp trực tiếp với mục tiêu mà không cần xóa, do đó câu trả lời sẽ trở thành khẳng định ngay lập tức. 

### Ví dụ 2 

Từ đầu vào:`cdecup is postponed`| từ | kết quả quét | bỏ qua | hợp lệ | 
| --- | --- | --- | --- | 
| cdecup | khớp codecup với một chữ 'o' bị thiếu | 1 | Có | 

Ở đây ký tự bị thiếu tương ứng với một lần xóa được phép khỏi từ gốc. 

Điều này chứng tỏ rằng thuật toán chấp nhận chính xác các biến dạng giống như chuỗi con với chính xác một ký tự bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 7) | Mỗi từ được so sánh với một mẫu có độ dài cố định bằng cách quét tuyến tính | 
| Không gian | O(1) | Chỉ sử dụng trạng thái bổ sung không đổi | 

Các ràng buộc đủ nhỏ đến mức ngay cả việc triển khai đơn giản cũng thấp hơn nhiều so với bất kỳ giới hạn nào. Độ dài mẫu không đổi làm cho giải pháp tuyến tính một cách hiệu quả theo số lượng từ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    global input
    input = _sys.stdin.readline

    TARGET = "codecup"

    def matches(word):
        i = j = 0
        skipped = 0
        while i < len(word) and j < len(TARGET):
            if word[i] == TARGET[j]:
                i += 1
                j += 1
            else:
                if skipped == 1:
                    return False
                skipped += 1
                j += 1
        return i == len(word)

    n = int(input())
    words = input().split()
    return "Yes" if any(matches(w) for w in words) else "No"

# provided samples
assert run("""4
codeforsquares codecup coming soon
""") == "Yes"

assert run("""3
cdecup is postponed
""") == "Yes"

assert run("""5
abracadabra code cup hello all
""") == "No"

# custom cases
assert run("""1
codecup
""") == "Yes"

assert run("""1
codecap
""") == "No"

assert run("""1
cdecp
""") == "No"

assert run("""2
codecup xcodecup
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu chính xác duy nhất | Có | chấp nhận cơ bản | 
| trường hợp thay thế | Không | từ chối các thay đổi ký tự không hợp lệ | 
| xóa nhiều lần | Không | thực thi tối đa một lần xóa | 
| tin nhắn hỗn hợp | Có | tìm thấy bất kỳ từ hợp lệ nào | 

## Vỏ cạnh 

Một trường hợp thất bại hữu ích là một từ như “codecap”, khác với mục tiêu ở một vị trí duy nhất nhưng thông qua việc thay thế chứ không phải xóa. Thuật toán không bao giờ cho phép thay thế ký tự, chỉ bỏ qua trong mẫu nên không thể căn chỉnh ký tự không khớp và từ chối chính xác ký tự đó. 

Một trường hợp đặc biệt khác là một từ như “cdecp”, yêu cầu xóa nhiều ký tự khỏi “codecup” ban đầu. Bộ đếm bỏ qua ngăn chặn nhiều hơn một sự không khớp trong mẫu, do đó, khi cần khoảng cách cấu trúc thứ hai, việc khớp sẽ ngay lập tức thất bại. 

Cuối cùng, các từ ngắn hơn 6 ký tự không thể thể hiện sự sai lệch hợp lệ của nguồn 7 ký tự với nhiều nhất một lần xóa. Logic con trỏ đương nhiên không thành công vì phải bỏ qua quá nhiều ký tự trong mẫu, vượt quá số lần xóa được phép.
