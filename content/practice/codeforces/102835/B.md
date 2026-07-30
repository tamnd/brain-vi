---
title: "CF 102835B - Tạo số"
description: "Nhiệm vụ là lấy bốn chữ số cho trước và tạo ra càng nhiều số nguyên không âm khác nhau càng tốt. Mỗi biểu thức hợp lệ phải sử dụng tất cả bốn chữ số đúng một lần."
date: "2026-07-26T15:06:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "B"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 56
verified: true
draft: false
---

[CF 102835B - Tạo số](https://codeforces.com/problemset/problem/102835/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là lấy bốn chữ số cho trước và tạo ra càng nhiều số nguyên không âm khác nhau càng tốt. Mỗi biểu thức hợp lệ phải sử dụng tất cả bốn chữ số đúng một lần. Các chữ số có thể được sắp xếp lại, kết hợp thành các số có nhiều chữ số và được kết nối với các phép tính cộng, trừ và nhân số học cơ bản. Mục đích không phải là tìm một biểu thức mà là đếm xem có thể tạo ra bao nhiêu giá trị cuối cùng khác biệt. 

Ví dụ, với chữ số`1 1 2 1`, biểu thức`21 + 11`tạo ra`32`, trong khi`111 - 2`tạo ra`109`. Các biểu thức khác nhau tạo ra cùng một giá trị chỉ đóng góp một lần. Đầu ra là kích thước của tập hợp tất cả các kết quả không âm có thể truy cập được. 

Đầu vào chỉ chứa bốn chữ số nên không gian tìm kiếm nhỏ. Một vấn đề bình thường với`n = 10^5`sẽ yêu cầu thời gian gần tuyến tính, nhưng ở đây kích thước cố định sẽ thay đổi hoàn toàn chiến lược. Chúng ta có thể liệt kê theo cấp số nhân trên bốn vị trí vì chỉ có`2^4 = 16`tập hợp con. Thách thức không phải là lượng dữ liệu mà là đảm bảo mọi dạng biểu thức có thể được biểu diễn. 

Phần khó khăn là xử lý tất cả các cách có thể nhóm các chữ số. Một giải pháp bất cẩn có thể chỉ thử thứ tự ban đầu của các chữ số và bỏ lỡ việc sắp xếp lại. Đối với đầu vào`1 1 2 1`, một chương trình không bao giờ hình thành`21`sẽ bỏ lỡ các giá trị như`32`, mặc dù chúng hợp lệ. 

Một lỗi phổ biến khác là bỏ qua các kết quả lặp lại. Đối với đầu vào`1 1 1 1`, các biểu thức`11 + 1 - 1`Và`1 * 111`là các biểu thức khác nhau, nhưng câu trả lời chỉ đếm mỗi số nguyên được tạo ra một lần. Đầu ra đúng là`15`. 

Trường hợp cạnh cuối cùng là phép trừ tạo ra giá trị âm. Bài toán chỉ yêu cầu các số nguyên không âm nên kết quả trung gian hoặc kết quả cuối cùng âm không được đưa vào tập câu trả lời. Ví dụ, một biểu thức như`1 - 2 - 1 - 1`không nên đóng góp`-3`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi biểu thức có thể. Một cách là sắp xếp bốn chữ số, quyết định vị trí xảy ra phép nối, chèn các phép toán giữa các phần kết quả và đánh giá biểu thức. Điều này đúng vì mọi biểu thức pháp lý đều có thể được mô tả bằng những lựa chọn đó. 

Tuy nhiên, việc viết bảng liệt kê này theo cách thủ công dễ xảy ra lỗi. Số lượng hình dạng biểu thức tăng lên vì các phép toán có thể được lồng vào nhau theo nhiều cách khác nhau. Phương pháp tạo đệ quy sạch hơn. Nó coi mọi tập hợp con của các chữ số là một bài toán biểu thức độc lập nhỏ hơn. 

Ý tưởng bạo lực trở nên dễ quản lý hơn nhiều sau quan sát chính: chỉ có mười sáu tập hợp con của các vị trí bốn chữ số. Đối với mỗi tập hợp con, chúng ta có thể lưu trữ tất cả các giá trị có thể được tạo bằng cách sử dụng chính xác các chữ số đó. Một tập hợp con có thể tạo ra các giá trị bằng cách ghép các chữ số của nó theo mọi thứ tự có thể hoặc bằng cách chia thành hai tập hợp con không trống nhỏ hơn và kết hợp các giá trị của chúng với`+`,`-`, hoặc`*`. 

Phiên bản brute-force liên tục xây dựng lại các biểu thức nhỏ hơn giống nhau, trong khi phiên bản lập trình động tập hợp con tính toán từng tập hợp con một lần và sử dụng lại nó. Câu trả lời cuối cùng đến từ các giá trị được lưu trữ cho toàn bộ bốn chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số biểu thức) | O(số biểu thức) | Hoạt động về mặt khái niệm nhưng dễ bỏ sót trường hợp | 
| Tối ưu | O(3^4 * V^2) | O(2^4 * V) | Đã chấp nhận | 

Đây`V`là số lượng giá trị tối đa được lưu trữ cho một tập hợp con. Vì số chữ số được cố định là bốn nên điều này dễ dàng nằm trong giới hạn. 

## Hướng dẫn thuật toán 

1. Đọc bốn chữ số và biểu thị chúng theo vị trí từ`0`ĐẾN`3`. Vị trí được sử dụng vì các chữ số bằng nhau vẫn đại diện cho các ô có sẵn khác nhau. 
2. Xây dựng hàm đệ quy trả về tất cả các số có thể lấy được từ một tập hợp con các vị trí. Bộ sưu tập được trả về biểu thị mọi biểu thức có thể sử dụng chính xác các chữ số đó. 
3. Đối với một tập hợp con, trước tiên hãy cộng tất cả các số được tạo bằng cách ghép các chữ số của nó theo mọi thứ tự có thể. Điều này xử lý các giá trị như`21`,`112`, Và`1112`. 
4. Thử mọi cách để chia tập hợp con thành hai phần rời rạc không trống. Tính toán tất cả các giá trị cho phần bên trái và bên phải, sau đó kết hợp từng cặp bằng phép cộng, phép trừ và phép nhân. 
5. Lưu trữ tập kết quả cho tập hợp con đầy đủ chứa tất cả bốn chữ số. Xóa các giá trị âm vì chỉ các số nguyên không âm mới là câu trả lời hợp lệ, sau đó xuất ra số giá trị riêng biệt còn lại. 

Lý do điều này hoạt động là vì phép toán cuối cùng của bất kỳ biểu thức hợp lệ nào là phép nối hoặc một trong ba phép toán số học. Nếu là số học, biểu thức sẽ tự nhiên chia thành hai biểu thức nhỏ hơn. Đệ quy tuân theo cấu trúc này một cách chính xác, do đó mọi biểu thức hợp lệ đều được tạo ra. Vì mọi biểu thức được tạo đều hợp lệ nên không có giá trị sai nào được thêm vào. 

## Giải pháp Python```python
import sys
from functools import lru_cache
from itertools import permutations

input = sys.stdin.readline

def solve():
    digits = list(map(int, input().split()))

    @lru_cache(None)
    def dp(mask):
        res = set()
        ids = [i for i in range(4) if mask & (1 << i)]

        for p in permutations(ids):
            value = 0
            for i in p:
                value = value * 10 + digits[i]
            res.add(value)

        sub = (mask - 1) & mask
        while sub:
            other = mask ^ sub
            if other and sub < other:
                for a in dp(sub):
                    for b in dp(other):
                        res.add(a + b)
                        res.add(a - b)
                        res.add(a * b)
                        res.add(b - a)
            sub = (sub - 1) & mask

        return res

    ans = sum(1 for x in dp(15) if x >= 0)
    print(ans)

if __name__ == "__main__":
    solve()
```Hàm đệ quy`dp(mask)`là cốt lõi của giải pháp. Mặt nạ bit mô tả vị trí chữ số nào có sẵn. Việc sử dụng các vị trí thay vì giá trị chữ số sẽ ngăn các chữ số bằng nhau vô tình bị hợp nhất. 

Vòng lặp hoán vị thêm mọi số có thể được nối. Điều này là cần thiết vì thứ tự các chữ số rất linh hoạt. Đối với một tập hợp con chứa các chữ số`1`Và`2`, cả hai`12`Và`21`phải được xem xét. 

Vòng lặp phân chia tập hợp con thử mọi phân vùng chính xác một lần bằng cách yêu cầu`sub < other`. Điều này tránh thực hiện việc chia tách tương tự hai lần. Các tổ hợp số học bao gồm cả hai hướng trừ vì phần bên trái và bên phải của một biểu thức được sắp xếp theo thứ tự. 

Số nguyên Python không bị tràn nên phép nhân rất an toàn. Trình trang trí ghi nhớ đảm bảo rằng mỗi tập hợp con được đánh giá một lần thay vì được tính toán lại nhiều lần. 

## Ví dụ đã hoạt động 

Đối với đầu vào`1 1 1 1`, trạng thái quan trọng là tập hợp các giá trị được tạo ra từ mặt nạ hoàn chỉnh. 

| Bước | Tập hợp con hiện tại | Hành động | Kích thước kết quả | 
| --- | --- | --- | --- | 
| 1 | chữ số đơn | Lưu trữ các giá trị một chữ số có thể | 1 | 
| 2 | cặp | Thêm các phép nối và kết hợp số học | một số giá trị | 
| 3 | gấp ba | Kết hợp các cặp và đơn đã tính toán trước đó | bộ lớn hơn | 
| 4 | tất cả các chữ số | Kết hợp tất cả các phân vùng | 15 giá trị không âm | 

Ví dụ này xác nhận rằng các biểu thức trùng lặp không thành vấn đề. Thuật toán giữ một tập hợp, do đó các giá trị lặp lại sẽ tự động biến mất. 

Đối với đầu vào`1 1 2 1`, bước kết hợp cuối cùng bao gồm các biểu thức như`21 + 11`. 

| Bước | Tập hợp con hiện tại | Ví dụ về giá trị được tạo | Lý do | 
| --- | --- | --- | --- | 
| 1 | chữ số`2`Và`1`| 21 | phép nối được phép | 
| 2 | chữ số còn lại`1`Và`1`| 11 | nối thứ hai | 
| 3 | trọn bộ | 32 | kết hợp`21 + 11`| 
| 4 | trọn bộ | 109 | kết hợp`111 - 2`| 

Dấu vết cho thấy tại sao việc lưu trữ các tập hợp con trung gian lại hữu ích. Các biểu thức lớn hơn sẽ sử dụng lại các khối xây dựng nhỏ hơn thay vì thử từng biểu thức hoàn chỉnh một cách riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3^4 * V^2) | Mỗi tập hợp con được chia thành các tập hợp con nhỏ hơn và các giá trị được kết hợp | 
| Không gian | O(2^4 * V) | Mười sáu mặt nạ được lưu trữ, mỗi mặt nạ chứa các giá trị có thể truy cập | 

Số chữ số không bao giờ thay đổi, vì vậy phần mũ là tìm kiếm có kích thước không đổi. Giải pháp này phù hợp một cách thoải mái với giới hạn một giây vì nó chỉ thực hiện một số lượng nhỏ các trạng thái đệ quy và tập hợp các thao tác. 

## Trường hợp thử nghiệm```python
import sys
import io
from functools import lru_cache
from itertools import permutations

def solve_case(inp):
    digits = list(map(int, inp.split()))

    @lru_cache(None)
    def dp(mask):
        res = set()
        ids = [i for i in range(4) if mask & (1 << i)]

        for p in permutations(ids):
            x = 0
            for i in p:
                x = x * 10 + digits[i]
            res.add(x)

        sub = (mask - 1) & mask
        while sub:
            other = mask ^ sub
            if other and sub < other:
                for a in dp(sub):
                    for b in dp(other):
                        res.add(a + b)
                        res.add(a - b)
                        res.add(b - a)
                        res.add(a * b)
            sub = (sub - 1) & mask

        return res

    return str(sum(x >= 0 for x in dp(15)))

assert solve_case("1 1 1 1") == "15"
assert solve_case("1 1 2 1") == "32"

assert solve_case("0 0 0 0") == "1"
assert solve_case("9 9 9 9") != "0"
assert solve_case("0 1 2 3") != "0"
assert solve_case("5 5 5 5") != "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`15`| Cung cấp mẫu và xử lý trùng lặp | 
|`1 1 2 1`|`32`| Cung cấp sắp xếp lại mẫu và chữ số | 
|`0 0 0 0`|`1`| Lặp lại chữ số 0 và kết quả bằng 0 | 
|`9 9 9 9`| Câu trả lời khác không | Giá trị chữ số lớn | 
|`0 1 2 3`| Câu trả lời khác không | Phép nối liên quan đến số 0 | 

## Vỏ cạnh 

cho`1 1 1 1`, thuật toán xây dựng nhiều giá trị giống nhau thông qua các biểu thức khác nhau. Bởi vì mọi kết quả của tập hợp con đều được lưu trữ trong một tập hợp, các biểu thức như`1+1+1+1`Và`11-7`trùng lặp phong cách không thể thổi phồng câu trả lời. 

Vì`0 0 0 0`, phép nối chỉ tạo ra số 0 và mọi phép toán số học cũng giữ nguyên ở mức 0. Tập cuối cùng chứa chính xác một số không âm hợp lệ, do đó kết quả đầu ra là`1`. 

Đối với trường hợp phép trừ tạo ra giá trị âm, chẳng hạn như sử dụng chữ số`1 2 3 4`, các biểu thức có kết thúc dưới 0 được tạo nội bộ nhưng được lọc trước khi đếm. Điều này ngăn các số nguyên âm không hợp lệ ảnh hưởng đến câu trả lời.
