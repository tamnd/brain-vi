---
title: "CF 48A - Máy kéo giấy"
description: "Ba người chơi đồng thời chọn một trong các cử chỉ oẳn tù tì cổ điển: \"đá\", \"giấy\" hoặc \"kéo\". Người chơi chỉ thắng nếu cử chỉ của họ đánh bại cử chỉ của cả hai người chơi khác."rock\", \"paper\", or \"scissors\". A player wins only if their gesture defeats the gestures shown by both other players."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "schedules"]
categories: ["algorithms"]
codeforces_contest: 48
codeforces_index: "A"
codeforces_contest_name: "School Personal Contest #3 (Winter Computer School 2010/11) - Codeforces Beta Round 45 (ACM-ICPC Rules)"
rating: 900
weight: 48
solve_time_s: 91
verified: true
draft: false
---
[CF 48A - Máy kéo giấy](https://codeforces.com/problemset/problem/48/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, tiến độ 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ba người chơi đồng thời chọn một trong những động tác oẳn tù tì cổ điển:`"rock"`,`"paper"`, hoặc`"scissors"`. Người chơi chỉ thắng nếu cử chỉ của họ đánh bại cử chỉ của cả hai người chơi khác. 

Điều đó có nghĩa là chỉ có người chiến thắng trong những tình huống sau: 

- Một người chơi ném đá trong khi hai người còn lại thể hiện kéo. 
- Một người chơi giơ kéo, hai người còn lại giơ giấy. 
- Một người chơi giơ giấy trong khi hai người còn lại giơ đá. 

Mọi cấu hình khác đều không tạo ra người chiến thắng. 

Đầu vào bao gồm chính xác ba chuỗi. Chiếc đầu tiên thuộc về chú Fyodor, chiếc thứ hai thuộc về Matroskin và chiếc thứ ba thuộc về Sharic. Đầu ra là một ký tự đơn đại diện cho người chiến thắng:`"F"`,`"M"`,`"S"`, hoặc`"?"`nếu không có ai thắng duy nhất. 

Các ràng buộc rất nhỏ vì luôn có chính xác ba người chơi và chỉ có ba cử chỉ có thể thực hiện được. Mọi giải pháp đều chạy ngay lập tức. Ngay cả việc kiểm tra mọi tương tác có thể có theo cách thủ công cũng sẽ mất nhiều thời gian. Thử thách thực sự là triển khai logic trò chơi một cách chính xác mà không bỏ sót trường hợp nào. 

Phần khó khăn là nhiều bang không có người chiến thắng. Việc thực hiện bất cẩn thường cho rằng phải luôn có một. 

Hãy xem xét đầu vào này:```
rock
paper
scissors
```Mỗi cử chỉ đều thắng và thua trước một cử chỉ khác. Không ai đánh bại được cả hai đối thủ nên câu trả lời đúng là:```
?
```Việc triển khai đơn giản chỉ kiểm tra chiến thắng theo cặp có thể tuyên bố không chính xác người chiến thắng ở đây. 

Một lỗi dễ dàng khác xuất hiện khi hai người chơi hòa nhau bằng cùng một động tác:```
paper
rock
rock
```Đầu ra đúng là:```
F
```Giấy đánh bại đá, và vì cả hai đối thủ đều thể hiện đá nên chú Fyodor đánh bại cả hai cùng một lúc. 

Tình huống ngược lại cũng phải được xử lý chính xác:```
rock
paper
paper
```Bây giờ câu trả lời đúng là:```
M
```Mặc dù có hai người chơi thể hiện cử chỉ chiến thắng nhưng vẫn không có người chiến thắng duy nhất theo luật. Người chơi giấy đầu tiên thắng vì trò chơi xác định từng người chơi chứ không phải cử chỉ chung. Vì Matroskin là người chơi duy nhất có nước đi đánh bại cả hai đối thủ nên anh ta là người chiến thắng. 

Cuối cùng, khi mọi người chọn cùng một cử chỉ:```
scissors
scissors
scissors
```không ai thắng nên kết quả là:```
?
```## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng trò chơi chính xác như mô tả. Đối với mỗi người chơi, hãy kiểm tra xem cử chỉ của họ có giống cử chỉ của cả hai người chơi khác hay không. Nếu đúng thì người chơi đó sẽ thắng. 

Vì chỉ có ba người chơi nên cách tiếp cận bạo lực này đã hoạt động hoàn hảo. Chúng ta có thể định nghĩa một quy tắc trợ giúp như: 

- kéo đập đá 
- kéo đập giấy 
- giấy đập đá 

Sau đó, với mỗi người chơi, hãy so sánh cử chỉ của họ với hai người còn lại. 

Tổng công là không đổi vì chúng ta chỉ thực hiện một số phép so sánh. Ngay cả việc triển khai ngây thơ nhất cũng dễ dàng nằm trong giới hạn. 

Nhận xét quan trọng là cấu trúc của oẳn tù tì khiến điều kiện thắng là cực kỳ nhỏ. Người chơi chỉ thắng nếu hai cử chỉ còn lại giống hệt nhau và đặc biệt dễ bị tổn thương trước cử chỉ của người chiến thắng. 

Ví dụ: 

- đá chỉ thắng được hai kéo 
- Kéo chỉ thắng được hai tờ giấy 
- giấy chỉ thắng được hai hòn đá 

Điều này cho phép chúng tôi đơn giản hóa việc triển khai hơn nữa. Thay vì mô phỏng mọi trận đấu, chúng ta có thể trực tiếp kiểm tra ba kiểu chiến thắng có thể xảy ra. 

Điều đó mang lại một giải pháp thời gian liên tục ngắn và rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba cử chỉ thành các biến đại diện cho Fyodor, Matroskin và Sharic. 
2. Tạo ánh xạ mô tả cử chỉ nào đánh bại cử chỉ nào: 

-`"rock"`đánh bại`"scissors"`-`"scissors"`đánh bại`"paper"`-`"paper"`đánh bại`"rock"`3. Kiểm tra xem Fyodor có đánh bại được cả hai người chơi khác hay không. 

Điều này đúng khi cử chỉ của Fyodor đánh bại cử chỉ của Matroskin và cũng đánh bại cử chỉ của Sharic. 
4. Nếu Fyodor thắng, hãy in`"F"`và dừng lại. 
5. Lặp lại thao tác kiểm tra tương tự cho Matroskin. 

Nếu cử chỉ của anh ta đánh bại cả hai đối thủ, hãy in`"M"`. 
6. Lặp lại thao tác kiểm tra tương tự cho Sharic. 

Nếu cử chỉ của anh ta đánh bại cả hai đối thủ, hãy in`"S"`. 
7. Nếu không có điều kiện nào trong ba điều kiện thành công, hãy in`"?"`. 

Điều này bao gồm các chu kỳ như oẳn tù tì và các trường hợp trong đó tất cả các cử chỉ đều bằng nhau. 

### Tại sao nó hoạt động 

Các quy tắc xác định người chiến thắng là người có cử chỉ đánh bại cả hai cử chỉ khác cùng một lúc. Thuật toán kiểm tra điều kiện chính xác đó một cách độc lập cho mỗi người chơi. 

Trò oẳn tù tì có kết quả xác định. Một cử chỉ có thể đánh bại một cử chỉ khác hoặc không. Vì mọi người chiến thắng có thể phải đáp ứng điều kiện được thuật toán kiểm tra và mọi điều kiện được kiểm tra đều khớp chính xác với quy tắc trò chơi nên thuật toán không thể bỏ sót người chiến thắng hợp lệ hoặc tạo ra một người chiến thắng không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

f = input().strip()
m = input().strip()
s = input().strip()

beats = {
    "rock": "scissors",
    "scissors": "paper",
    "paper": "rock"
}

if beats[f] == m and beats[f] == s:
    print("F")
elif beats[m] == f and beats[m] == s:
    print("M")
elif beats[s] == f and beats[s] == m:
    print("S")
else:
    print("?")
```Chương trình bắt đầu bằng cách đọc ba cử chỉ theo thứ tự cố định mà câu lệnh yêu cầu. sử dụng`.strip()`là cần thiết vì dòng đầu vào chứa các ký tự dòng mới. 

các`beats`từ điển mã hóa trực tiếp các quy tắc trò chơi. Ví dụ:```
beats["rock"] == "scissors"
```nghĩa là đá thắng kéo. 

Mỗi điều kiện kiểm tra xem một người chơi có đánh bại cả hai đối thủ hay không. Ví dụ:```
beats[f] == m and beats[f] == s
```hỏi liệu cử chỉ của Fyodor có đồng thời đánh bại cử chỉ của Matroskin và cử chỉ của Sharic hay không. 

Thứ tự kiểm tra không quan trọng vì nhiều nhất một người chơi có thể đáp ứng điều kiện thắng. Nếu không có điều kiện nào phù hợp thì trò chơi không có người chiến thắng duy nhất, do đó chương trình sẽ in ra`"?"`. 

Một điểm tinh tế là các mối quan hệ được xử lý một cách tự nhiên. Giả sử đầu vào là:```
paper
rock
rock
```Sau đó:```
beats["paper"] == "rock"
```đúng cho cả hai đối thủ nên Fyodor thắng chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
rock
rock
rock
```| Biến | Giá trị | 
| --- | --- | 
| f | đá | 
| m | đá | 
| s | đá | 

| Kiểm tra | Kết quả | 
| --- | --- | 
| đá đập đá | Sai | 
| Không có người chơi nào đánh bại được cả hai đối thủ | Đúng | 

Đầu ra:```
?
```Ví dụ này cho thấy trường hợp hoàn toàn bằng nhau. Không có cử chỉ nào tự đánh bại chính mình, vì vậy không ai có thể đánh bại cả hai đối thủ. 

### Ví dụ 2 

đầu vào:```
paper
rock
rock
```| Biến | Giá trị | 
| --- | --- | 
| f | giấy | 
| m | đá | 
| s | đá | 

| Kiểm tra | Kết quả | 
| --- | --- | 
| giấy đập đá | Đúng | 
| giấy lại đập đá | Đúng | 
| Fyodor thắng | Đúng | 

Đầu ra:```
F
```Dấu vết này thể hiện mô hình chiến thắng quan trọng: một người chơi thực hiện động tác đánh bại các cử chỉ giống hệt nhau của cả hai đối thủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một số lượng so sánh cố định | 
| Không gian | O(1) | Chương trình chỉ lưu trữ ba chuỗi và một từ điển nhỏ | 

Kích thước đầu vào không bao giờ tăng, do đó thời gian chạy và mức sử dụng bộ nhớ không đổi. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    f = input().strip()
    m = input().strip()
    s = input().strip()

    beats = {
        "rock": "scissors",
        "scissors": "paper",
        "paper": "rock"
    }

    if beats[f] == m and beats[f] == s:
        print("F")
    elif beats[m] == f and beats[m] == s:
        print("M")
    elif beats[s] == f and beats[s] == m:
        print("S")
    else:
        print("?")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run("rock\nrock\nrock\n") == "?\n", "sample 1"

# custom cases
assert run("paper\nrock\nrock\n") == "F\n", "Fyodor wins"
assert run("rock\npaper\nrock\n") == "M\n", "Matroskin wins"
assert run("rock\nrock\npaper\n") == "S\n", "Sharic wins"
assert run("rock\npaper\nscissors\n") == "?\n", "cyclic case"
assert run("scissors\nscissors\nscissors\n") == "?\n", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`paper rock rock`|`F`| Người chơi đầu tiên đánh bại hai đối thủ ngang nhau | 
|`rock paper rock`|`M`| Người chơi thứ hai chiến thắng | 
|`rock rock paper`|`S`| Người chơi thứ ba chiến thắng | 
|`rock paper scissors`|`?`| Cấu hình không có người chiến thắng theo chu kỳ | 
|`scissors scissors scissors`|`?`| Cấu hình hoàn toàn bằng nhau | 

## Vỏ cạnh 

Hãy xem xét cấu hình tuần hoàn:```
rock
paper
scissors
```Thuật toán kiểm tra từng người chơi một cách riêng biệt. 

Fyodor dùng đá, đánh bại kéo nhưng lại thua giấy nên không thể thắng. 

Matroskin sử dụng giấy, đánh được đá nhưng lại thua kéo. 

Sharic sử dụng kéo, đánh bại giấy nhưng thua đá. 

Vì không ai đánh bại được cả hai đối thủ nên thuật toán đi đến nhánh cuối cùng và in:```
?
```Bây giờ hãy xem xét một cử chỉ thua trùng lặp:```
paper
rock
rock
```Thuật toán đầu tiên kiểm tra Fyodor:```
beats["paper"] == "rock"
```Điều này đúng cho cả hai đối thủ, vì vậy câu trả lời trở thành:```
F
```Các lần kiểm tra còn lại sẽ bị bỏ qua vì đã tìm thấy người chiến thắng hợp lệ. 

Cuối cùng, hãy coi tất cả các cử chỉ đều bình đẳng:```
rock
rock
rock
```Mọi so sánh đều thất bại vì đá không đánh bại được đá. Không có điều kiện nào trong ba điều kiện chiến thắng trở thành đúng, do đó thuật toán sẽ đưa ra kết quả chính xác:```
?
```
