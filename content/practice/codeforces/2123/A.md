---
title: "CF 2123A - Trò chơi bảng đen"
description: "Bảng đen ban đầu chứa mọi số nguyên từ 0 đến n - 1. Một vòng chơi luôn có hai nước đi. Đầu tiên Alice loại bỏ một số số a. Sau đó, Bob phải loại bỏ một số b khác sao cho $$a+b tương đương với 3 pmod 4.$$ Nếu Bob không thể tìm thấy số đó, trò chơi sẽ kết thúc ngay lập tức và Bob thua."
date: "2026-06-08T03:36:36+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2123
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1034 (Div. 3)"
rating: 800
weight: 2123
solve_time_s: 97
verified: true
draft: false
---

[CF 2123A - Trò chơi bảng đen](https://codeforces.com/problemset/problem/2123/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bảng đen ban đầu chứa mọi số nguyên từ`0`ĐẾN`n - 1`. 

Một hiệp đấu luôn có hai nước đi. Đầu tiên Alice loại bỏ một số số`a`. Sau đó Bob phải xóa một số khác`b`như vậy$$a+b \equiv 3 \pmod 4.$$Nếu Bob không tìm được số đó, trò chơi sẽ kết thúc ngay lập tức và Bob thua cuộc. Sau khi Bob xóa số của mình, vòng tiếp theo sẽ bắt đầu. Người chơi tiếp tục cho đến khi ai đó không thể thực hiện được bước đi cần thiết. 

Chúng ta được cho một số giá trị của`n`và đối với mỗi người, chúng ta phải xác định người chơi nào thắng nếu cả hai đều chơi hoàn hảo. 

Hạn chế là rất nhỏ. Có tối đa 100 trường hợp thử nghiệm và`n ≤ 100`. Việc tìm kiếm trò chơi bạo lực có thể khả thi nhưng mục tiêu thực sự là khám phá cấu trúc toán học đằng sau trò chơi. Khi đã hiểu được cấu trúc đó, câu trả lời cho từng trường hợp kiểm thử có thể được tính toán theo thời gian không đổi. 

Điều tinh tế là Bob không chọn một con số tùy ý. Sự lựa chọn của anh ta hoàn toàn được xác định bởi lớp thặng dư của số modulo 4 của Alice. 

Các cặp hợp lệ là:$$0 \leftrightarrow 3$$Và$$1 \leftrightarrow 2.$$Số nào cũng bằng`0 (mod 4)`phải khớp với một số tương ứng với`3 (mod 4)`, và bất kỳ số nào đồng dạng với`1 (mod 4)`phải khớp với một số tương ứng với`2 (mod 4)`. 

Một sai lầm phổ biến là tập trung vào các giá trị chính xác thay vì phần dư của chúng theo modulo 4. Ví dụ: khi`n = 5`, các số đó là:$$0,1,2,3,4.$$số`4`cư xử chính xác như`0`vì cả hai đều đồng dạng với`0 (mod 4)`. 

Một sai lầm dễ mắc phải khác là cho rằng một số chẵn tổng số nguyên sẽ tự động nghiêng về Bob. Vì`n = 2`, có hai số, nhưng tập hợp đó là`{0,1}`. Không có đối tác hợp lệ nào tồn tại cho cả hai lựa chọn, vì vậy Alice thắng ngay lập tức. 

đầu vào:```
1
2
```Đầu ra:```
Alice
```Bob thua ngay lần đi đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lập mô hình tập hợp các số còn lại hiện tại và thử đệ quy mọi động thái hợp pháp. Alice sẽ cố gắng đạt đến trạng thái chiến thắng, trong khi Bob sẽ cố gắng tránh thua cuộc. 

Điều này hoạt động vì không gian trạng thái nhỏ khi`n ≤ 100`, nhưng nó hoàn toàn bỏ qua cấu trúc của trò chơi. Ngay cả đối với mức độ vừa phải`n`, số lượng trạng thái trò chơi có thể có sẽ tăng theo cấp số nhân. 

Quan sát quan trọng là chỉ có dư lượng vật chất modulo 4. 

Mỗi số thuộc về một trong bốn nhóm:$$0,\ 1,\ 2,\ 3 \pmod 4.$$Phản hồi của Bob phải đến từ một nhóm bổ sung cụ thể: 

| Alice chọn | Bob phải chọn | 
| --- | --- | 
| 0 mod 4 | 3 mod 4 | 
| 3 mod 4 | 0 mod 4 | 
| 1 mod 4 | 2 mod 4 | 
| 2 mod 4 | 1 mod 4 | 

Điều này có nghĩa là trò chơi chia thành hai nhóm hoàn toàn độc lập: 

1. Dư lượng`0`Và`3`. 
2. Dư lượng`1`Và`2`. 

Cho phép$$c_0,c_1,c_2,c_3$$là số đếm có mỗi dư lượng. 

Một nước đi trong nhóm đầu tiên luôn loại bỏ một phần tử khỏi`c_0`và một từ`c_3`. 

Một nước đi trong nhóm thứ hai luôn loại bỏ một phần tử khỏi`c_1`và một từ`c_2`. 

Bob có thể tiếp tục phản hồi miễn là cả hai số lượng trong nhóm đã chọn vẫn dương. 

Đối với những con số`0`bởi vì`n-1`, số lượng dư lượng khác nhau nhiều nhất là một. Kiểm tra một vài giá trị đầu tiên sẽ thấy mẫu: 

| n | Người chiến thắng | 
| --- | --- | 
| 1 | Alice | 
| 2 | Alice | 
| 3 | Alice | 
| 4 | Bob | 
| 5 | Alice | 
| 6 | Alice | 
| 7 | Alice | 
| 8 | Bob | 

Mỗi khối bốn số đều đóng góp đúng một`(0,3)`cặp và một cái hoàn chỉnh`(1,2)`đôi. 

Khi`n`là bội số của 4, mọi lớp dư lượng đều xuất hiện với tần suất như nhau. Tất cả các số có thể được phân chia thành các cặp hợp lệ. Dù Alice có làm gì thì Bob vẫn luôn có phản hồi. Sau khi loại bỏ hết các số, Alice là người chơi đầu tiên không thể di chuyển nên Bob thắng. 

Khi`n`không phải là bội số của 4, một hoặc nhiều lớp dư lượng có phần tử phụ. Alice cuối cùng có thể chọn từ phần còn lại chưa từng có và để Bob mà không có phản hồi pháp lý. Bob không thể ngăn chặn điều này vì sự mất cân bằng đã tồn tại ở vị trí ban đầu. 

Người chiến thắng chỉ đơn giản là: 

- Bob nếu`n % 4 == 0`- Alice thì ngược lại 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Về mặt khái niệm quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Tính toán`n % 4`. 
3. Nếu phần còn lại là`0`, đầu ra`"Bob"`. 

Trong trường hợp này, mọi lớp dư lượng xuất hiện thường xuyên như nhau, vì vậy mọi số đều thuộc về một cặp khớp hợp lệ nào đó. 
4. Nếu không thì xuất ra`"Alice"`. 

Ít nhất một loại dư lượng là chưa từng có. Alice có thể khai thác sự mất cân bằng này và cuối cùng buộc Bob vào một vị trí không có phản ứng pháp lý. 

### Tại sao nó hoạt động 

Trò chơi hoàn toàn được xác định bởi số lượng dư lượng modulo 4. Các nước đi hợp lệ luôn tiêu tốn một số từ một cặp lớp dư lượng bổ sung, hoặc`(0,3)`hoặc`(1,2)`. 

Khi`n`chia hết cho 4 thì số lượng của cả 4 loại dư lượng đều bằng nhau. Mọi phần tử đều có thể khớp nhau, vì vậy Bob luôn có phản ứng trước nước đi của Alice cho đến khi bảng trống. Alice sau đó phải đối mặt với vị trí đầu tiên mà không có động thái hợp pháp và thua cuộc. 

Khi`n`không chia hết cho 4 thì ít nhất một lớp dư có nhiều phần tử hơn lớp bổ sung của nó. Những yếu tố bổ sung đó không bao giờ có thể phù hợp. Alice có thể buộc chơi cho đến khi chỉ còn lại những phần tử chưa từng có, lúc đó Bob không thể phản hồi. Do đó Alice thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    print("Bob" if n % 4 == 0 else "Alice")
```Việc thực hiện tuân theo đặc tính toán học một cách trực tiếp. 

Giá trị duy nhất quan trọng là`n % 4`. Nếu phần còn lại bằng 0 thì các lớp dư lượng hoàn toàn cân bằng và Bob thắng. Mọi phần còn lại đều tạo ra sự mất cân bằng có lợi cho Alice. 

Không cần mô phỏng. Không có trạng thái trò chơi cần phải được lưu trữ. Mỗi trường hợp thử nghiệm được xử lý độc lập trong thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Số lượng dư lượng: 

| Dư lượng | Đếm | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Tất cả các lớp đều được cân bằng. 

| Bước | Giá trị | 
| --- | --- | 
| n | 4 | 
| n% 4 | 0 | 
| Người chiến thắng | Bob | 

Mỗi số thuộc về một cặp hợp lệ. Bob luôn có thể trả lời nước đi của Alice cho đến khi bảng trống. 

### Ví dụ 2 

đầu vào:```
5
```Các số là:```
0 1 2 3 4
```Số lượng dư lượng: 

| Dư lượng | Đếm | 
| --- | --- | 
| 0 | 2 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 

Phần tử dư lượng-0 bổ sung không có đối tác phù hợp lâu dài. 

| Bước | Giá trị | 
| --- | --- | 
| n | 5 | 
| n% 4 | 1 | 
| Người chiến thắng | Alice | 

Dấu vết này thể hiện sự mất cân bằng quan trọng. Một phần tử không thể được ghép nối, điều này cuối cùng khiến Bob không có phản hồi hợp pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một hoạt động mô-đun cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có bộ nhớ phụ | 

Các ràng buộc là cực kỳ nhỏ, nhưng giải pháp thậm chí còn đơn giản hơn. Mỗi trường hợp thử nghiệm yêu cầu công việc liên tục và sử dụng bộ nhớ không đổi, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        ans.append("Bob" if n % 4 == 0 else "Alice")
    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run("5\n2\n4\n5\n7\n100\n") == \
       "Alice\nBob\nAlice\nAlice\nBob"

# minimum n
assert run("1\n1\n") == "Alice"

# first Bob position
assert run("1\n4\n") == "Bob"

# just above multiple of 4
assert run("1\n9\n") == "Alice"

# maximum constraint
assert run("1\n100\n") == "Bob"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`| Alice | Bảng nhỏ nhất có thể | 
|`n = 4`| Bob | Cấu hình cân bằng đầu tiên | 
|`n = 9`| Alice | Bội của 4 cộng một | 
|`n = 100`| Bob | Giá trị ràng buộc tối đa | 

## Vỏ cạnh 

cho`n = 1`, bảng chỉ chứa`{0}`. 

đầu vào:```
1
1
```Alice loại bỏ`0`. Không còn số nào. Bob không có phản hồi pháp lý. 

Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n | 1 | 
| n% 4 | 1 | 

Đầu ra:```
Alice
```Vì`n = 2`, bảng chứa`{0,1}`. 

đầu vào:```
1
2
```Không có số nào có sẵn dư lượng bổ sung. Alice loại bỏ thứ gì thì Bob sẽ thua ngay lập tức. 

Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n | 2 | 
| n% 4 | 2 | 

Đầu ra:```
Alice
```Vì`n = 8`, số lượng dư lượng được cân bằng hoàn hảo: 

| Dư lượng | Đếm | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 

đầu vào:```
1
8
```Mọi phần tử đều có thể được khớp. Bob luôn có câu trả lời cho đến khi bảng trống. 

Thuật toán tính toán: 

| Biến | Giá trị | 
| --- | --- | 
| n | 8 | 
| n% 4 | 0 | 

Đầu ra:```
Bob
```Những trường hợp này bao gồm bàn cờ nhỏ nhất, Bob thua ngay lập tức và vị trí hoàn toàn cân bằng khi Bob thắng. Quy tắc`n % 4 == 0`xử lý chính xác tất cả chúng.
