---
title: "CF 106196A - \u041a\u0440\u0443\u0433\u043b\u0430\u044f \u043a\u0430\u0440\u0442\u0430"
description: "Nhiệm vụ mô tả một quyết định trong trò chơi bài Durak. Chúng tôi được trao bộ bài tẩy và hai quân bài khác nhau. Lá bài đầu tiên được dùng làm lá bài tấn công và chúng ta cần quyết định xem nó có thể che được lá bài thứ hai hay không. Một lá bài có thể che phủ một lá bài khác trong đúng hai trường hợp."
date: "2026-06-25T10:34:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106196
codeforces_index: "A"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2025"
rating: 0
weight: 106196
solve_time_s: 32
verified: true
draft: false
---

[CF 106196A - \u041a\u0440\u0443\u0433\u043b\u0430\u044f \u043a\u0430\u0440\u0442\u0430](https://codeforces.com/problemset/problem/106196/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một quyết định trong trò chơi bài Durak. Chúng tôi được trao bộ bài tẩy và hai quân bài khác nhau. Lá bài đầu tiên được dùng làm lá bài tấn công và chúng ta cần quyết định xem nó có thể che được lá bài thứ hai hay không. 

Một lá bài có thể che phủ một lá bài khác trong đúng hai trường hợp. Nếu cả hai quân bài có cùng chất, quân bài đầu tiên sẽ thắng khi thứ hạng của nó cao hơn. Nếu quân bài đầu tiên thuộc bộ bài tẩy còn quân bài thứ hai không thuộc quân bài tẩy thì quân bài đầu tiên cũng thắng bất kể thứ hạng. Tất cả các kết hợp khác đều thua. 

Đầu vào chứa một ký tự đơn cho bộ bài tẩy, theo sau là hai mô tả thẻ. Mỗi lá bài có hai ký tự: ký tự đầu tiên là cấp bậc của nó và ký tự thứ hai là chất của nó. Đầu ra là`YES`khi lá bài đầu tiên che lá bài thứ hai, nếu không thì là`NO`. 

Các ràng buộc rất nhỏ vì chỉ có hai thẻ và không cần xử lý lặp lại. Việc mô phỏng, sắp xếp hoặc bất kỳ cấu trúc dữ liệu nâng cao nào sẽ không cần thiết. Lời giải chỉ cần một vài so sánh, thời gian liên tục là đủ. 

Những sai lầm chính đến từ việc nhầm lẫn giữa hai quy tắc hoặc quên thứ tự của chúng. Một cách tiếp cận sai lầm phổ biến là so sánh thứ hạng trước khi kiểm tra chất bài, điều này cho phép một quân bài không phải là quân át chủ bài mạnh đánh bại quân bài tẩy một cách không chính xác. 

Ví dụ:```
Input:
H
AS KH

Output:
NO
```Việc thực hiện bất cẩn có thể thấy điều đó`A`cao hơn`K`và trả lời`YES`, nhưng chất bài thì khác nhau và quân bài đầu tiên không phải là quân bài tẩy. Thẻ thứ hai không thể được bảo hiểm. 

Một trường hợp khác là khi cả hai quân bài đều là quân bài tẩy.```
Input:
S
7S 6S

Output:
YES
```Quy tắc Trump không cần phải được sử dụng ở đây. Đơn giản là các quân bài có chất giống nhau và việc so sánh thứ hạng sẽ quyết định kết quả. 

Trường hợp cạnh cuối cùng là khi quân bài đầu tiên là quân bài tẩy còn quân bài thứ hai thì không, ngay cả khi quân bài tẩy có thứ hạng nhỏ hơn.```
Input:
C
6C AS

Output:
YES
```Chỉ riêng việc so sánh thứ hạng sẽ thất bại vì`6`thấp hơn`A`, nhưng quy tắc át chủ bài sẽ ghi đè lên cấp bậc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là cố gắng mô hình hóa toàn bộ logic trò chơi, kiểm tra các cách mà các lá bài có thể tương tác và tìm kiếm thông qua tất cả các quy tắc. Điều này vẫn sẽ tạo ra kết quả chính xác vì trò chơi chỉ có một so sánh để thực hiện, nhưng nó giải quyết được một vấn đề lớn hơn nhiều so với mức cần thiết. Ngay cả một mô phỏng nhỏ cũng bị lãng phí ở đây vì câu trả lời chỉ phụ thuộc vào hai quân bài đã cho và bộ bài tẩy. 

Quan sát quan trọng là mối quan hệ bao phủ có một định nghĩa trực tiếp. Chúng ta không cần khám phá các khả năng. Chúng ta chỉ cần chuyển hai điều kiện chiến thắng sang dạng kiểm tra boolean. 

Ý tưởng bạo lực thất bại ở chỗ nó làm tăng thêm sự phức tạp không cần thiết. Nhận xét rằng chỉ có hai lý do độc lập để một lá bài đánh bại lá bài khác sẽ làm giảm vấn đề thành một số lượng so sánh ký tự không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số trạng thái trò chơi mô phỏng) | O(số trạng thái mô phỏng) | Quá phức tạp cho nhiệm vụ | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bộ bài tẩy và mô tả hai lá bài. Chia mỗi thẻ thành cấp bậc và chất riêng để có thể so sánh hai thuộc tính một cách độc lập. 
2. Kiểm tra xem quân bài đầu tiên có phải là quân bài tẩy hay không, còn quân bài thứ hai thì không. Nếu điều này đúng, quân bài đầu tiên sẽ che ngay quân bài thứ hai vì quân bài tẩy luôn đánh bại quân bài không phải quân bài tẩy. 
3. Nếu điều kiện của quân bài tẩy không quyết định được câu trả lời, hãy kiểm tra xem cả hai quân bài có cùng chất hay không. Khi các chất phù hợp với nhau, hãy so sánh thứ hạng của chúng bằng cách sử dụng thứ tự xếp hạng đã cho. 
4. Đầu ra`YES`nếu thứ hạng của lá bài đầu tiên cao hơn trong cùng một bộ đồ. Nếu không thì xuất ra`NO`. 

Tại sao nó hoạt động: thuật toán tuân theo định nghĩa đầy đủ của mối quan hệ bao phủ. Các trường hợp thắng duy nhất là quân bài tẩy đánh bại quân bài không phải quân bài tẩy hoặc hai quân bài cùng chất mà quân bài đầu tiên có thứ hạng cao hơn. Vì mọi cặp bài có thể đều thỏa mãn một trong những điều kiện này hoặc không thỏa mãn nên kết quả luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    trump = input().strip()
    a, b = input().split()

    ranks = "6789TJQKA"

    a_rank, a_suit = a[0], a[1]
    b_rank, b_suit = b[0], b[1]

    if a_suit == trump and b_suit != trump:
        print("YES")
    elif a_suit == b_suit and ranks.index(a_rank) > ranks.index(b_rank):
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Chuỗi`ranks`lưu trữ các thẻ theo thứ tự tăng dần. sử dụng`index`chuyển đổi một ký tự xếp hạng thành một số có thể so sánh trực tiếp. Ví dụ,`K`xuất hiện sau`Q`, do đó chỉ số của nó lớn hơn. 

Việc kiểm tra át chủ bài được thực hiện đầu tiên vì nó không phụ thuộc vào cấp bậc. Một quân bài tẩy thấp vẫn có thể đánh bại một quân bài không phải là quân bài tẩy cao, vì vậy việc so sánh thứ hạng trước khi kiểm tra quân bài tẩy sẽ gây ra lỗi. 

Điều kiện thứ hai xử lý các lá bài cùng loại. Vì đầu vào đảm bảo các thẻ là khác nhau nên sự bình đẳng về thứ hạng không thể xảy ra giữa hai thẻ, nhưng việc so sánh chặt chẽ cũng khớp chính xác với các quy tắc. 

Việc triển khai chỉ sử dụng một vài biến và không bao giờ tạo thêm bộ sưu tập, do đó không có mối lo ngại về ranh giới hoặc bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
H
QH 9S
```Thẻ đầu tiên là`QH`, thứ hai là`9S`. 

| Bước | Trump | Thẻ đầu tiên | Thẻ thứ hai | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | H | QH | 9S | Lá bài đầu tiên là át chủ bài và lá bài thứ hai không phải | Đúng | 
| 2 | H | QH | 9S | Che phủ thành công | CÓ | 

Lá bài đầu tiên là trái tim, là quân bài tẩy, còn lá bài thứ hai thì không. So sánh thứ hạng là không liên quan. 

### Mẫu 2 

đầu vào:```
S
8D 6D
```| Bước | Trump | Thẻ đầu tiên | Thẻ thứ hai | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | S | 8D | 6D | Lá bài đầu tiên là át chủ bài và lá bài thứ hai không phải | Sai | 
| 2 | S | 8D | 6D | Cùng bộ đồ và cấp bậc cao hơn | Đúng | 
| 3 | S | 8D | 6D | Câu trả lời cuối cùng | CÓ | 

Ví dụ này xác nhận sự so sánh thông thường về bộ đồ giống nhau. Con át chủ bài không quan trọng vì cả hai quân bài đều không phải là quân át chủ bài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một số lần so sánh ký tự và tra cứu thứ hạng cố định | 
| Không gian | O(1) | Chỉ một số chuỗi và biến được lưu trữ | 

Giải pháp dễ dàng nằm trong giới hạn vì công việc không phụ thuộc vào bất kỳ kích thước đầu vào lớn nào. Nó thực hiện cùng một lượng tính toán cho mọi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    trump = sys.stdin.readline().strip()
    a, b = sys.stdin.readline().split()

    ranks = "6789TJQKA"

    a_rank, a_suit = a[0], a[1]
    b_rank, b_suit = b[0], b[1]

    if a_suit == trump and b_suit != trump:
        ans = "YES"
    elif a_suit == b_suit and ranks.index(a_rank) > ranks.index(b_rank):
        ans = "YES"
    else:
        ans = "NO"

    sys.stdin = old_stdin
    return ans

assert solve_data("H\nQH 9S\n") == "YES", "sample 1"
assert solve_data("S\n8D 6D\n") == "YES", "sample 2"
assert solve_data("C\n7H AS\n") == "NO", "sample 3"

assert solve_data("C\n6C AS\n") == "YES", "trump beats stronger non-trump"
assert solve_data("H\n6S AH\n") == "NO", "non-trump cannot beat trump"
assert solve_data("D\nAD KD\n") == "YES", "same suit highest rank"
assert solve_data("S\n6H 7H\n") == "NO", "same suit lower rank"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`C / 6C AS`| CÓ | Quy tắc của Trump có tác dụng ngay cả với cấp bậc thấp hơn | 
|`H / 6S AH`| KHÔNG | Quân bài không phải át chủ bài không thể đánh bại quân bài tẩy | 
|`D / AD KD`| CÓ | So sánh thứ hạng giống nhau cao nhất | 
|`S / 6H 7H`| KHÔNG | Thẻ cùng chất dưới không thành công | 

## Vỏ cạnh 

Đối với trường hợp con át chủ bài và không phải con át chủ bài:```
Input:
C
6C AS
```Thuật toán trước tiên sẽ kiểm tra xem quân bài đầu tiên có phải là quân bài tẩy hay không.`6C`có bộ đồ Trump và`AS`không, vì vậy nó ngay lập tức quay trở lại`YES`. Nó không bao giờ so sánh`6`Và`A`, điều này tránh được sai lầm phổ biến là từ chối một nước đi át chủ bài hợp lệ. 

Đối với trường hợp quân bài thứ hai là quân bài tẩy:```
Input:
H
6S AH
```Lá bài đầu tiên không phải là quân bài tẩy và các chất khác nhau. Điều kiện đầu tiên không thành công và điều kiện tương tự cũng thất bại. Thuật toán trả về`NO`, phù hợp với quy tắc rằng quân bài không phải là quân bài tẩy không thể che được quân bài tẩy. 

Đối với những lá bài giống nhau:```
Input:
D
AD KD
```Cả hai thẻ đều có kim cương. Bộ bài tẩy không liên quan vì quân bài đầu tiên đã thắng nhờ có thứ hạng cao hơn trong cùng bộ. Việc tra cứu thứ hạng mang lại`A`một vị trí lớn hơn`K`, vậy câu trả lời là`YES`. 

Đối với trường hợp cùng bộ đồ cấp thấp hơn:```
Input:
S
6H 7H
```Cả hai lá bài đều là trái tim, nhưng thứ hạng của lá bài đầu tiên nhỏ hơn. Thuật toán đạt đến sự so sánh giống nhau và trả về`NO`, ngăn chặn một câu trả lời sai do chỉ kiểm tra xem các bộ đồ có khớp hay không.
