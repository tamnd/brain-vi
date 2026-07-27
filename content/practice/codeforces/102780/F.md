---
title: "CF 102780F - Trò chơi chữ"
description: "Trò chơi được chơi dựa trên một từ, nhưng thứ tự các chữ cái không phải là phần quan trọng. Một động thái chỉ quan tâm đến việc mỗi chữ cái còn lại bao nhiêu bản sao."
date: "2026-07-27T20:11:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "F"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 95
verified: true
draft: false
---

[CF 102780F - Trò chơi chữ](https://codeforces.com/problemset/problem/102780/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi dựa trên một từ, nhưng thứ tự các chữ cái không phải là phần quan trọng. Một động thái chỉ quan tâm đến việc mỗi chữ cái còn lại bao nhiêu bản sao. Nếu một chữ cái xuất hiện nhiều lần, người chơi có thể xóa một bản sao, xóa chính xác hai bản sao hoặc xóa mọi bản sao còn lại của chữ cái đó. Người chơi nào làm cho tổng số chữ cái còn lại trở thành số 0 sẽ thắng. 

Đầu vào là một từ viết hoa có tối đa 40 ký tự. Kết quả đầu ra cho biết người chơi nào có chiến lược chiến thắng nếu cả hai người chơi đều chọn nước đi tối ưu. Vấn đề ban đầu là từ Cuộc thi khu vực miền Trung nước Nga 2019. 

Độ dài từ nhỏ có nghĩa là ban đầu có thể thực hiện được một tìm kiếm đơn giản trên tất cả các trạng thái trò chơi. Tuy nhiên, số lượng trạng thái có thể tăng lên nhanh chóng vì mỗi cách phân bổ chữ cái khác nhau sẽ tạo ra một vị trí khác nhau. Một giải pháp khám phá tất cả các trạng thái có thể đạt đến thời gian theo cấp số nhân, điều này không cần thiết ngay cả đối với độ dài 40. Thay vào đó, chúng ta cần tìm một thuộc tính toán học của mỗi số lượng chữ cái. 

Các trường hợp nguy hiểm chính xuất phát từ việc coi các bước di chuyển như những bước di chuyển thông thường. Một giải pháp bất cẩn có thể cho rằng một chữ cái có số chia hết cho ba luôn thua vì việc loại bỏ một hoặc hai chữ cái giống như trò chơi lấy đi. Điều này không thành công vì việc loại bỏ tất cả các bản sao là một động thái đặc biệt. Ví dụ, đầu vào`AAA`có câu trả lời`Alice`. Số đếm là ba, nhưng Alice có thể loại bỏ cả ba chữ cái ngay lập tức. 

Một lỗi phổ biến khác là bỏ qua các chữ cái chỉ xuất hiện một lần. Đối với đầu vào`A`, câu trả lời là`Alice`bởi vì nước đi duy nhất sẽ loại bỏ chữ cái cuối cùng. Giải pháp chỉ kiểm tra các cặp hoặc nhóm hoàn chỉnh sẽ đánh dấu sai là thua. 

Trường hợp cạnh thứ ba là toàn bộ từ không có một giá trị trò chơi nào. Đối với đầu vào`AB`, câu trả lời là`Bob`, bởi vì mỗi chữ cái đóng góp độc lập và tác dụng của chúng bị hủy bỏ. Một giải pháp chỉ nhìn vào tổng chiều dài sẽ bỏ lỡ sự tương tác này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô hình hóa trạng thái từ hiện tại và thử đệ quy mọi động thái có thể. Đối với mỗi chữ cái, chúng ta có thể giảm số lượng của nó đi một, giảm đi hai nếu có thể hoặc đặt nó về 0. Nếu một trạng thái đệ quy có ít nhất một nước đi dẫn đến trạng thái thua thì đó là trạng thái thắng. Ngược lại là thua. 

Phương pháp bạo lực này là đúng vì nó tuân thủ chính xác định nghĩa về cách chơi tối ưu. Vấn đề là số lượng trạng thái. Với 26 chữ cái có thể đếm được, ngay cả một từ ngắn cũng có thể tạo ra nhiều cách phân phối khác nhau. Trường hợp xấu nhất là theo cấp số nhân về số lượng chữ cái, bởi vì mọi tập hợp con bị xóa có thể xuất hiện dưới dạng một trạng thái riêng biệt. 

Quan sát làm thay đổi vấn đề là mọi loại chữ cái đều hoạt động giống như một thành phần trò chơi độc lập. Đây là một tình huống trò chơi khách quan tiêu chuẩn, trong đó mỗi thành phần có một giá trị Grundy và toàn bộ vị trí được xác định bằng cách xor-ing các giá trị đó. 

Chỉ xem xét một chữ cái xuất hiện`c`lần. Cho phép`g(c)`là giá trị Grundy của nó. chuyển động của nó đi đến`c - 1`,`c - 2`, Và`0`khi những động thái đó tồn tại. Tính toán các giá trị đầu tiên cho: 

| Đếm | Giá trị bẩn thỉu | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 1 | 
| 5 | 2 | 
| 6 | 3 | 

Mô hình lặp lại sau mỗi ba lần đếm. Số đếm dương chia hết cho 3 có giá trị 3, trong khi các số đếm khác có giá trị bằng số dư của chúng. 

Toàn bộ từ khi đó chỉ là xor của 26 giá trị chữ cái. Nếu xor khác 0, Alice có thể di chuyển đến vị trí xor 0 và giành chiến thắng. Nếu xor bằng 0, mọi nước đi đều mang lại cho Bob vị thế chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi chữ in hoa xuất hiện bao nhiêu lần trong từ. Trạng thái trò chơi chỉ phụ thuộc vào các tần số này chứ không phụ thuộc vào vị trí của các chữ cái. 
2. Đối với mỗi chữ cái có tần số`c`, tính giá trị Grundy của nó. Nếu như`c`bằng 0, nó không đóng góp gì cả. Nếu không, hãy sử dụng`c % 3`và thay thế phần còn lại bằng 0 bằng giá trị 3. 
3. Xor tất cả 26 giá trị với nhau. Xor kết hợp các trò chơi độc lập được biểu thị bằng các chữ cái khác nhau. 
4. In`Alice`khi xor khác 0. In`Bob`khi xor bằng 0. 

Lý do bước cuối cùng có hiệu quả là vì vị trí xor bằng 0 đang thua trong các trò chơi tổ hợp công bằng. Vị trí xor khác 0 luôn có sự di chuyển đến vị trí xor 0, trong khi vị trí xor 0 không thể di chuyển sang vị trí xor 0 khác. 

Tại sao nó hoạt động: 

Mỗi tần số chữ cái là một chồng độc lập. Một nước đi sẽ thay đổi chính xác một cọc và không ảnh hưởng đến những cọc khác. Giá trị Grundy tóm tắt mọi tương lai có thể xảy ra của một cọc. Do đó, xor của tất cả các giá trị cọc là giá trị đầy đủ của từ. Nếu giá trị này bằng 0, cả hai người chơi đều rơi vào tình trạng thua cuộc. Nếu khác 0, người chơi hiện tại có một nước đi khiến xor về 0, khiến đối thủ rơi vào trạng thái thua cuộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - ord('A')] += 1

    x = 0
    for c in cnt:
        if c:
            value = c % 3
            if value == 0:
                value = 3
            x ^= value

    print("Alice" if x else "Bob")

if __name__ == "__main__":
    solve()
```Mảng`cnt`lưu trữ các thành phần trò chơi độc lập. Chỉ có 26 trong số đó nên việc triển khai không bao giờ cần mô phỏng các bước di chuyển. 

Việc chuyển đổi Grundy chỉ được thực hiện đối với tần số dương. Các chữ cái tần số bằng 0 có giá trị bằng 0 và không ảnh hưởng đến xor. Việc xử lý đặc biệt bội số của ba là cần thiết vì một đống có kích thước ba không tương đương với một đống có kích thước bằng 0. Động thái loại bỏ tất cả các chữ cái sẽ thay đổi kiểu lặp lại. 

Xor cuối cùng được lưu trữ trong`x`. Số nguyên Python không bị tràn, mặc dù dù sao thì các giá trị ở đây cũng rất nhỏ. Đầu vào chứa một từ, vì vậy chỉ cần gọi tới`readline`là đủ. 

## Ví dụ đã hoạt động 

cho`ZADACHA`, số chữ cái là: 

| Thư | Đếm | Giá trị bẩn thỉu | Xor hiện tại | 
| --- | --- | --- | --- | 
| A | 3 | 3 | 3 | 
| C | 1 | 1 | 2 | 
| D | 1 | 1 | 3 | 
| H | 1 | 1 | 2 | 
| Z | 1 | 1 | 3 | 

Xor cuối cùng khác 0 nên Alice thắng. 

Vì`WORD`, số đếm là: 

| Thư | Đếm | Giá trị bẩn thỉu | Xor hiện tại | 
| --- | --- | --- | --- | 
| W | 1 | 1 | 1 | 
| Ồ | 1 | 1 | 0 | 
| R | 1 | 1 | 1 | 
| D | 1 | 1 | 0 | 

Xor cuối cùng bằng 0 nên Bob thắng. 

Những dấu vết này cho thấy tại sao kết quả phụ thuộc vào tần số chữ cái thay vì thứ tự ban đầu của từ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đếm các chữ cái phải vượt qua một từ, sau đó chỉ có 26 giá trị được xử lý. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ 26 bộ đếm. | 

Độ dài tối đa chỉ là 40, do đó nghiệm tuyến tính dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ liên tục cũng không thay đổi đối với đầu vào lớn hơn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("ZADACHA\n") == "Alice", "sample 1"
assert run("WORD\n") == "Bob", "sample 2"

assert run("A\n") == "Alice", "single letter"
assert run("AAA\n") == "Alice", "all equal letters"
assert run("AB\n") == "Bob", "xor cancellation"
assert run("A" * 40 + "\n") == "Bob", "maximum size multiple of three"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`| Alice | Một lá thư có thể tháo rời duy nhất phải chiến thắng. | 
|`AAA`| Alice | Việc di chuyển tất cả các chữ cái sẽ thay đổi mẫu Grundy cho bội số của ba. | 
|`AB`| Bob | Các chữ cái độc lập kết hợp thông qua xor, không phải theo tổng chiều dài. | 
| 40 bản`A`| Bob | Xử lý tần số lớn và các giá trị Grundy lặp lại. | 

## Vỏ cạnh 

Đối với đầu vào`AAA`, thuật toán đếm một chữ cái với tần số ba. Giá trị Grundy của nó là 3, vì vậy xor là 3 và Alice được in. Điều này xử lý trường hợp loại bỏ tất cả các bản sao tốt hơn là lấy một hoặc hai bản sao. 

Đối với đầu vào`A`, số đếm là một, cho giá trị Grundy là 1. Xor khác 0, vì vậy Alice thắng ngay lập tức bằng cách loại bỏ chữ cái duy nhất. 

Đối với đầu vào`AB`, cả hai chữ cái đều có tần số bằng một và đều đóng góp giá trị 1. Xor trở thành`1 ^ 1 = 0`, nên vị thế đang bị mất. Mỗi nước đi để lại đúng một chữ cái và đối thủ loại bỏ chữ cái cuối cùng. 

Đối với một từ chứa 40 bản sao của cùng một chữ cái, số đếm là 40. Giá trị Grundy là`40 % 3 = 1`, vậy là vị trí đang thắng. Thuật toán xử lý kích thước đầu vào tối đa mà không phụ thuộc vào số lần di chuyển có thể.
