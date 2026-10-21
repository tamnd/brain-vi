---
title: "CF 105819A - Miếng hoa huệ"
description: "Có $N$ hàng hoa huệ. Mỗi hàng có chính xác hai miếng đệm, trái và phải. Bất cứ lúc nào cũng có chính xác một trong số chúng đang nổi. Hai con ếch xuất phát trước hàng đầu tiên và cả hai đều phải đến được bờ bên kia."
date: "2026-06-25T15:05:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "A"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 44
verified: true
draft: false
---

[CF 105819A - Miếng đệm hoa huệ](https://codeforces.com/problemset/problem/105819/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có$N$hàng lá huệ. Mỗi hàng có chính xác hai miếng đệm, trái và phải. Bất cứ lúc nào cũng có chính xác một trong số chúng đang nổi. 

Hai con ếch xuất phát trước hàng đầu tiên và cả hai đều phải đến được bờ bên kia. Một con ếch chỉ có thể di chuyển về phía trước, từ hàng$i$chèo thuyền$i+1$và chỉ có thể hạ cánh trên bệ nổi hiện tại. Khi một con ếch rời khỏi một tấm đệm, hàng đó sẽ chuyển sang trạng thái: tấm đệm nó để lại chìm xuống và tấm đệm kia nổi lên. Mỗi con ếch kiếm được một điểm bất cứ khi nào nó đáp xuống ô bên phải. Chúng ta có thể chọn thứ tự của tất cả các lần nhảy và muốn tổng số điểm tối đa mà hai con ếch kiếm được. 

Đầu vào cung cấp phần đệm nổi ban đầu ở mỗi hàng. Đối với hàng$i$,`L`có nghĩa là miếng đệm bên trái bắt đầu nổi và`R`có nghĩa là miếng đệm bên phải bắt đầu nổi.$N$nhiều nhất là$100$, do đó, ngay cả một mô phỏng cũng sẽ không đáng kể nếu xét từ góc độ hiệu suất. Nhiệm vụ thực sự là nhận ra sự bất biến ẩn trong hành vi lật. 

Một sai lầm phổ biến là nghĩ rằng thứ tự nhảy có thể thay đổi số lần hạ cánh ở bên phải liên tiếp. Coi như:```
1
R
```Con ếch đầu tiên ghé vào hàng phải đáp xuống ô bên phải và được điểm. Sau khi nó rời đi, hàng này bị lật nên con ếch thứ hai phải đáp xuống tấm đệm bên trái. Tổng số chính xác là 1. 

Tương tự:```
1
L
```Con ếch đầu tiên đáp xuống bên trái, hàng lật ngược và con ếch thứ hai đáp xuống bên phải. Tổng số lại là 1. 

Quan sát quan trọng là mỗi hàng được viếng thăm chính xác hai lần, mỗi con ếch ghé thăm một lần và hai lần ghé thăm buộc phải sử dụng các cạnh đối diện. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô hình hóa toàn bộ quá trình. Ở mỗi bước, chúng tôi chọn con ếch nào sẽ nhảy tiếp theo, theo dõi hàng nào đang bị chiếm giữ và theo dõi phía nổi hiện tại của mỗi hàng. Điều này khám phá chính xác tất cả các lịch trình hợp lệ, nhưng số lượng lệnh nhảy có thể tăng theo cấp số nhân và nhanh chóng trở nên không thực tế. 

Lý do mà việc sử dụng vũ lực là không cần thiết là vì các hàng hoàn toàn độc lập với quan điểm tính điểm. 

Tập trung vào một hàng duy nhất. Con ếch đầu tiên đến hàng phải đáp xuống phía nổi ban đầu. Trước khi con ếch thứ hai có thể sử dụng hàng đó, con ếch thứ nhất phải rời khỏi hàng đó, vì một tấm đệm chỉ có thể chứa được một con ếch. Khi con ếch đầu tiên rời đi, hàng sẽ bị đảo ngược. Kết quả là con ếch thứ hai buộc phải đáp xuống phía đối diện. 

Điều đó có nghĩa là mỗi hàng đóng góp chính xác một lần hạ cánh ở phần đệm bên phải: 

Nếu hàng bắt đầu như`R`, lượt truy cập đầu tiên là bên phải và lượt truy cập thứ hai là bên trái. 

Nếu hàng bắt đầu như`L`, lượt truy cập đầu tiên là bên trái và lượt truy cập thứ hai là bên phải. 

Dù thế nào đi nữa, trong số hai lần hạ cánh ở hàng đó, có đúng một lần hạ cánh ở bệ bên phải. 

Vì có$N$hàng và mỗi hàng đóng góp chính xác một điểm, câu trả lời luôn là$N$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$N$. 
2. Đọc chuỗi mô tả phía nổi ban đầu của mỗi hàng. 
3. Bỏ qua nội dung của chuỗi. Câu trả lời chỉ phụ thuộc vào số lượng hàng. 
4. Đầu ra$N$. 

Tại sao nó hoạt động: 

Ở mỗi hàng, hai con ếch phải ghé thăm đúng một lần. Chuyến thăm đầu tiên sử dụng phía hiện đang nổi. Sau khi con ếch đó rời khỏi hàng, hàng đó bị lật, buộc người thứ hai phải sử dụng mặt đối diện. Hai lượt thăm luôn bao gồm một lần hạ cánh bên trái và một lần hạ cánh bên phải. Do đó, mỗi hàng đóng góp chính xác một điểm, bất kể thứ tự nhảy hay cạnh ban đầu. Tổng hợp tất cả$N$hàng cho chính xác$N$điểm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

print(n)
```Việc thực hiện ngắn gọn vì toàn bộ vấn đề được đưa về bất biến hàng đã được chứng minh ở trên. 

Chuỗi đầu vào vẫn được đọc vì nó là một phần của định dạng đầu vào. Bỏ qua nó là cố ý. Liệu một hàng có bắt đầu như`L`hoặc`R`, đóng góp của nó luôn luôn chính xác là một điểm. 

Không cần mô phỏng. Không có trường hợp đặc biệt nào yêu cầu xử lý đặc biệt ngoài việc đọc chính xác dữ liệu đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
R
```| Hàng | Mặt ban đầu | Cuộc đổ bộ đầu tiên | Hạ cánh thứ hai | Điểm Từ Hàng | 
| --- | --- | --- | --- | --- | 
| 1 | R | R | L | 1 | 

Tổng số câu trả lời: 

| N | Trả lời | 
| --- | --- | 
| 1 | 1 | 

Ví dụ này cho thấy rằng một hàng bắt đầu bằng phần đệm bên phải nổi sẽ đóng góp một điểm ngay trong lần truy cập đầu tiên. 

### Ví dụ 2 

đầu vào:```
3
LLR
```| Hàng | Mặt ban đầu | Cuộc đổ bộ đầu tiên | Hạ cánh thứ hai | Điểm Từ Hàng | 
| --- | --- | --- | --- | --- | 
| 1 | L | L | R | 1 | 
| 2 | L | L | R | 1 | 
| 3 | R | R | L | 1 | 

Tổng số câu trả lời: 

| N | Trả lời | 
| --- | --- | 
| 3 | 3 | 

Điều này chứng tỏ rằng cấu hình ban đầu không thành vấn đề. Mỗi hàng đóng góp chính xác một điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sau khi đọc đầu vào, câu trả lời chỉ đơn giản là`n`| 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Các ràng buộc rất nhỏ nhưng giải pháp thậm chí còn đơn giản hơn quét tuyến tính. Thời gian chạy và mức sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    n = int(input())
    s = input().strip()
    print(n)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run("1\nR\n") == "1\n", "sample 1"

# custom cases
assert run("1\nL\n") == "1\n", "single row starting left"
assert run("2\nLR\n") == "2\n", "mixed configuration"
assert run("5\nRRRRR\n") == "5\n", "all right"
assert run("100\n" + "L" * 100 + "\n") == "100\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / R`|`1`| Trường hợp mẫu | 
|`1 / L`|`1`| Hàng đơn bắt đầu từ bên trái | 
|`2 / LR`|`2`| Trạng thái hàng hỗn hợp | 
|`5 / RRRRR`|`5`| Tất cả các hàng bắt đầu đúng | 
|`100 / L...L`|`100`| Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
L
```Con ếch đầu tiên đáp xuống miếng đệm bên trái. Khi nó rời đi, hàng lật. Con ếch thứ hai phải đáp xuống ô bên phải. Hàng đóng góp chính xác một điểm, vì vậy câu trả lời là 1. 

Bây giờ hãy xem xét:```
1
R
```Con ếch đầu tiên đáp xuống ô bên phải và được một điểm. Sau khi lật, con ếch thứ hai đáp xuống miếng đệm bên trái. Một lần nữa, hàng đóng góp chính xác một điểm, vì vậy câu trả lời là 1. 

Một ví dụ lớn hơn:```
4
LRRL
```Hàng 1 đóng góp một điểm, hàng 2 đóng góp một điểm, hàng 3 đóng góp một điểm và hàng 4 đóng góp một điểm. Tổng số là 4. Thứ tự nhảy có thể thay đổi con ếch nào nhận được một điểm cụ thể, nhưng nó không thể thay đổi tổng số lần hạ cánh ở bệ bên phải ở bất kỳ hàng nào. Bất biến vẫn còn nguyên cho mỗi hàng một cách độc lập.
