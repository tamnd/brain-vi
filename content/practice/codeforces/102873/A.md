---
title: "CF 102873A - Bắt kẻ mạo danh"
description: "Vấn đề mô tả một nhóm gồm n người chơi trong đó có chính xác một người chơi là kẻ mạo danh ẩn giấu. Chúng tôi được quan sát những người chơi đã hoàn thành nhiệm vụ. Người chơi có tên trong danh sách quan sát không thể là kẻ mạo danh."
date: "2026-07-25T13:10:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102873
codeforces_index: "A"
codeforces_contest_name: "Unofficial Div 4 Round #2 by ssense  SlavicG"
rating: 0
weight: 102873
solve_time_s: 47
verified: true
draft: false
---

[CF 102873A - Bắt kẻ mạo danh](https://codeforces.com/problemset/problem/102873/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một nhóm`n`người chơi trong đó có chính xác một người chơi là kẻ mạo danh ẩn giấu. Chúng tôi được quan sát những người chơi đã hoàn thành nhiệm vụ. Người chơi có tên trong danh sách quan sát không thể là kẻ mạo danh. Danh sách có thể chứa cùng một người chơi nhiều lần vì việc thấy ai đó thực hiện một số nhiệm vụ sẽ không cung cấp thêm bất kỳ thông tin nào. 

Mục đích là để xác định xem các quan sát có để lại chính xác một kẻ mạo danh có thể hay không. Nếu có chính xác một người chơi chưa bao giờ được nhìn thấy đang thực hiện một nhiệm vụ thì câu trả lời là`YES`. Nếu có thể có hai hoặc nhiều kẻ mạo danh, phi hành đoàn không thể chắc chắn và câu trả lời là`NO`. Có một trường hợp đặc biệt: nếu mọi người chơi xuất hiện trong danh sách, thông tin sẽ không nhất quán vì kẻ mạo danh chắc chắn đã làm giả một nhiệm vụ và vấn đề không cho phép chúng tôi cho rằng điều đó đã xảy ra. Câu trả lời cũng là`NO`. 

Các ràng buộc là nhỏ, tối đa là cả số lượng người chơi và số lượng quan sát`1000`. Điều này có nghĩa là ngay cả các giải pháp sử dụng quét tuyến tính đơn giản cũng đủ nhanh. Một giải pháp xung quanh`O(n + k)`chỉ thực hiện vài nghìn thao tác. Các cách tiếp cận phức tạp hơn như sắp xếp nhiều lần hoặc kiểm tra liên tục tất cả người chơi là không cần thiết, mặc dù chúng vẫn có thể vượt qua các giới hạn này. 

Các trường hợp khó khăn chính đến từ sự xuất hiện khó hiểu với những người chơi độc đáo. Các quan sát trùng lặp không làm giảm số lượng kẻ mạo danh có thể xảy ra vì một người chơi được nhìn thấy mười lần vẫn chỉ là một người chơi được xác nhận là không mạo danh. 

Ví dụ:```
3 3
1 2 2
```Đầu ra đúng là:```
YES
```Người chơi`1`Và`2`đã xác nhận an toàn nên chỉ có người chơi`3`có thể là kẻ mạo danh. Việc triển khai bất cẩn đếm số lần quan sát thay vì tính những người chơi riêng biệt có thể cho rằng đã tìm thấy ba người chơi và trả lời sai`NO`. 

Một trường hợp khác là khi mọi người chơi đều xuất hiện:```
3 5
2 1 1 3 3
```Đầu ra đúng là:```
NO
```Cả ba người chơi đều được nhìn thấy đang thực hiện nhiệm vụ. Vì kẻ mạo danh thường không thể hoàn thành nhiệm vụ nên không có người chơi nào mà chúng tôi có thể xác định một cách chắc chắn. 

Trường hợp ranh giới cuối cùng là khi có đúng hai người chơi:```
2 2
2 2
```Đầu ra đúng là:```
YES
```Chỉ người chơi`2`bị loại khỏi vai trò là kẻ mạo danh, khiến người chơi`1`như khả năng duy nhất. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực đơn giản sẽ coi mọi người chơi đều có thể là kẻ mạo danh. Đối với mỗi ứng cử viên, chúng tôi có thể kiểm tra xem người chơi đó có xuất hiện trong danh sách nhiệm vụ hay không. Nếu người chơi xuất hiện thì điều đó là không thể. Nếu không, chúng tôi coi chúng là một câu trả lời có thể. Điều này đúng vì thông tin duy nhất chúng tôi có là liệu một cầu thủ có từng được quan sát hay không. 

Vấn đề với phương pháp này là việc quét lặp đi lặp lại. Có tới`1000`người chơi và`1000`quan sát, vì vậy trường hợp xấu nhất thực hiện khoảng`1,000,000`séc. Điều này vẫn sẽ trôi qua ở đây, nhưng nó đang giải quyết một vấn đề tổng quát hơn mức cần thiết. 

Quan sát quan trọng là chúng tôi không quan tâm người chơi xuất hiện bao nhiêu lần. Chúng ta chỉ cần biết người chơi nào đã xuất hiện ít nhất một lần. Sau khi những người chơi đó được đánh dấu, câu trả lời chỉ được xác định bằng số lượng người chơi vẫn chưa được đánh dấu. 

Cách tiếp cận bạo lực có hiệu quả vì nó hỏi "người chơi này có thể làm được không?" cho từng người chơi riêng lẻ, nhưng nhận xét rằng tất cả người chơi đều độc lập cho phép chúng ta trả lời tất cả các ứng cử viên cùng một lúc. Một mảng boolean ghi lại xem từng người chơi có được nhìn thấy hay không, giảm bớt vấn đề về việc đếm những người chơi không nhìn thấy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(1) | Được chấp nhận ở đây, nhưng không cần thiết | 
| Tối ưu | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng ghi lại xem mỗi người chơi có được nhìn thấy đang thực hiện một nhiệm vụ hay không. Ban đầu mọi người chơi đều được coi là vô hình. 
2. Đọc mọi người chơi được quan sát và đánh dấu người chơi đó là đã xem. Việc xuất hiện nhiều lần không thay đổi được gì vì chúng tôi chỉ quan tâm đến việc người chơi có xuất hiện ít nhất một lần hay không. 
3. Đếm số người chơi vẫn chưa được nhìn thấy. Những người chơi này chính xác là những ứng cử viên có thể trở thành kẻ mạo danh. 
4. Nếu số lượng kẻ mạo danh có thể có chính xác là một, hãy in`YES`. Nếu không thì in`NO`. 

Lý do điều này có tác dụng là vì mọi người chơi được nhìn thấy sẽ bị loại khỏi danh sách xem xét và mọi người chơi không được nhìn thấy vẫn có thể tham gia. Phi hành đoàn chỉ có thể xác định được kẻ mạo danh khi tập còn lại có đúng một người chơi. 

Tại sao nó hoạt động: 

Thuật toán duy trì tính bất biến rằng sau khi xử lý bất kỳ tiền tố nào của danh sách quan sát, những người chơi được đánh dấu chính xác là những người chơi đã xuất hiện trong tiền tố đó. Cuối cùng, mọi người chơi không thể là kẻ mạo danh đều bị đánh dấu. Những người chơi duy nhất còn lại là những người phù hợp với tất cả các quan sát. Nếu có một người chơi như vậy, kẻ mạo danh sẽ được xác định duy nhất. Nếu không có hoặc có nhiều ứng cử viên thì sự chắc chắn là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    seen = [False] * (n + 1)

    for _ in range(k):
        x = int(input().strip()) if False else None

    # Re-read input handling the second line containing all values
    data = list(map(int, input().split()))
    for x in data:
        seen[x] = True

    possible = 0
    for i in range(1, n + 1):
        if not seen[i]:
            possible += 1

    print("YES" if possible == 1 else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng mảng boolean được lập chỉ mục theo số người chơi. Vì số lượng người chơi bắt đầu từ`1`, mảng có kích thước`n + 1`sao cho chỉ mục khớp trực tiếp với mã định danh người chơi. 

Sau khi đọc quan sát, mỗi người chơi trong danh sách sẽ được đánh dấu. Các giá trị trùng lặp không yêu cầu xử lý đặc biệt vì việc gán`True`nhiều lần có tác dụng tương tự như việc gán nó một lần. 

Vòng lặp cuối cùng đếm những người chơi không được đánh dấu. Sự so sánh với`1`là điều kiện cần thiết duy nhất vì cả 0 ứng cử viên và nhiều ứng cử viên đều có nghĩa là không thể xác định được kẻ mạo danh. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 3
1 2 2
```| Bước | Người chơi đã xử lý | Đã thấy mảng | Những kẻ mạo danh có thể | 
| --- | --- | --- | --- | 
| Bắt đầu | Không có | [Sai, Sai, Sai] | 3 | 
| 1 | Người chơi 1 | [Đúng, Sai, Sai] | 2 | 
| 2 | Người chơi 2 | [Đúng, Đúng, Sai] | 1 | 
| 2 lần nữa | Người chơi 2 | [Đúng, Đúng, Sai] | 1 | 

Người chơi duy nhất không được đánh dấu là người chơi`3`, do đó thuật toán in`YES`. Điều này chứng tỏ tại sao những quan sát trùng lặp sẽ không ảnh hưởng đến câu trả lời. 

Đối với mẫu thứ hai:```
4 2
3 1
```| Bước | Người chơi đã xử lý | Đã thấy mảng | Những kẻ mạo danh có thể | 
| --- | --- | --- | --- | 
| Bắt đầu | Không có | [Sai, Sai, Sai, Sai] | 4 | 
| 3 | Người chơi 3 | [Sai, Sai, Đúng, Sai] | 3 | 
| 1 | Người chơi 1 | [Đúng, Sai, Đúng, Sai] | 2 | 

Người chơi`2`Và`4`vẫn có thể. Vì có nhiều hơn một ứng cử viên nên kết quả là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Mỗi quan sát được xử lý một lần và mỗi người chơi được kiểm tra một lần | 
| Không gian | O(n) | Mảng boolean lưu trữ xem mỗi người chơi có được quan sát hay không | 

Các ràng buộc cho phép cách tiếp cận này một cách thoải mái. Với nhiều nhất`1000`người chơi, mức sử dụng bộ nhớ rất nhỏ và thời gian chạy thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n, k = map(int, sys.stdin.readline().split())
    seen = [False] * (n + 1)
    for x in map(int, sys.stdin.readline().split()):
        seen[x] = True

    cnt = sum(1 for i in range(1, n + 1) if not seen[i])
    print("YES" if cnt == 1 else "NO")

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("3 3\n1 2 2\n") == "YES\n", "sample 1"
assert run("4 2\n3 1\n") == "NO\n", "sample 2"
assert run("3 5\n2 1 1 3 3\n") == "NO\n", "sample 3"
assert run("2 2\n2 2\n") == "YES\n", "sample 4"

assert run("2 1\n1\n") == "YES\n", "minimum observations"
assert run("5 5\n1 2 3 4 4\n") == "YES\n", "duplicate values"
assert run("6 6\n1 2 3 4 5 6\n") == "NO\n", "all players seen"
assert run("10 1\n7\n") == "NO\n", "many possible impostors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1`|`YES`| Số lượng người chơi nhỏ nhất và ứng cử viên duy nhất còn lại | 
|`5 5 / 1 2 3 4 4`|`YES`| Quan sát trùng lặp không thành vấn đề | 
|`6 6 / 1 2 3 4 5 6`|`NO`| Tất cả người chơi được nhìn thấy đều không hợp lệ | 
|`10 1 / 7`|`NO`| Nhiều thí sinh còn lại không xác định được kẻ mạo danh | 

## Vỏ cạnh 

Khi quan sát chứa các bản sao, thuật toán chỉ thay đổi trạng thái của người chơi từ không nhìn thấy sang nhìn thấy một lần. 

Vì:```
3 3
1 2 2
```người chơi`2`được xử lý hai lần, nhưng trạng thái nhìn thấy vẫn giữ nguyên sau lần xuất hiện đầu tiên. Người chơi chưa được nhìn thấy còn lại là`3`, vì vậy đầu ra là`YES`. 

Khi mọi người chơi xuất hiện ít nhất một lần:```
3 5
2 1 1 3 3
```mảng nhìn thấy cuối cùng đánh dấu mọi người chơi. Số lượng kẻ mạo danh có thể trở thành số không. Vì không có người chơi nào có thể được xác định là kẻ mạo danh nên thuật toán sẽ đưa ra`NO`. 

Khi chỉ có một người chơi không được nhìn thấy:```
2 2
2 2
```thuật toán đánh dấu cầu thủ`2`và rời khỏi người chơi`1`không được đánh dấu. Số lượng kẻ mạo danh có thể chính xác là một, vì vậy câu trả lời là`YES`. Trường hợp này khẳng định số lượng quan sát không quan trọng, chỉ số lượng người chơi khác biệt bị loại.
