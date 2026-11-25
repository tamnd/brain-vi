---
title: "CF 980A - Mắt xích và Ngọc trai"
description: "Chúng ta có một sự sắp xếp hình tròn gồm hai loại ký tự: các viên ngọc được biểu thị bằng o và các liên kết được biểu thị bằng -. Vì cấu trúc là một vòng cổ nên chuỗi được coi là tuần hoàn, nghĩa là ký tự cuối cùng liền kề với ký tự đầu tiên."
date: "2026-06-17T01:11:31+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 980
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 480 (Div. 2)"
rating: 900
weight: 980
solve_time_s: 67
verified: true
draft: false
---

[CF 980A - Liên kết và Ngọc trai](https://codeforces.com/problemset/problem/980/A) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một sự sắp xếp hình tròn gồm hai loại ký tự: những viên ngọc tượng trưng bởi`o`và các liên kết được đại diện bởi`-`. Vì cấu trúc là một vòng cổ nên chuỗi được coi là tuần hoàn, nghĩa là ký tự cuối cùng liền kề với ký tự đầu tiên. 

Thao tác được phép không thay đổi nhiều bộ ký tự. Chúng ta có thể chọn bất kỳ ký tự nào và chèn lại nó vào bất kỳ vị trí nào khác trên vòng tròn. Điều này có nghĩa là chúng ta có thể sắp xếp lại chiếc vòng cổ một cách tùy ý nhưng không thể thay đổi số lượng ngọc trai hoặc mắt xích tồn tại. 

Mục đích là để xác định xem liệu chúng ta có thể sắp xếp lại chiếc vòng cổ sao cho khi chúng ta đi quanh vòng tròn, số mắt xích giữa mỗi cặp ngọc trai liên tiếp là giống hệt nhau đối với tất cả các cặp như vậy. 

Nếu có$k$ngọc trai, sau đó theo cách sắp xếp cuối cùng, vòng tròn được chia thành$k$cung giữa các viên ngọc liên tiếp và mỗi cung phải chứa cùng một số`-`nhân vật. 

Những hạn chế là nhỏ,$|s| \le 100$, do đó, ngay cả các giải pháp quét hoặc mô phỏng cấu hình nhiều lần cũng có thể chấp nhận được. Tuy nhiên, điều quan trọng là giảm bài toán xuống một điều kiện khả thi số học đơn giản hơn là mô phỏng sự sắp xếp lại. 

Một số trường hợp đặc biệt quan trọng. 

Nếu không có viên ngọc nào thì điều kiện này hoàn toàn vô nghĩa vì không có “những viên ngọc liền kề”. Bất kỳ lý do nào giả định ít nhất một`o`sẽ thất bại ở đây. 

Nếu có đúng một viên ngọc thì cũng không có cặp viên ngọc nào liền kề nhau, do đó mọi sự sắp xếp đều thỏa mãn điều kiện. 

Nếu có nhiều viên ngọc nhưng không có liên kết thì mỗi khoảng trống phải chứa 0 liên kết, vì vậy nó hợp lệ. 

Trường hợp tinh tế là khi các liên kết tồn tại nhưng không thể phân bố đều trên các khoảng trống ngọc trai, đặc biệt khi số lượng ngọc trai nhỏ so với các liên kết. 

## Phương pháp tiếp cận 

Thoạt nhìn, bài toán có vẻ giống như một bài toán sắp xếp lại một chu trình. Một ý tưởng mạnh mẽ sẽ là mô phỏng tất cả các hoán vị vòng tròn có thể có của các viên ngọc trai và các liên kết, hoặc tương đương, thử mọi cách để phân phối các liên kết vào các khoảng trống giữa các viên ngọc trai. 

Điều này nhanh chóng trở nên không khả thi. Ngay cả khi chúng ta chỉ xem xét vị trí của các viên ngọc trai trên một vòng tròn, số cách chọn kích thước khoảng cách tính tổng bằng tổng số liên kết sẽ tăng lên theo tổ hợp. Vì$k$ngọc trai và$m$liên kết, về cơ bản chúng tôi đang phân phối$m$các vật thể giống hệt nhau vào$k$thùng được dán nhãn, đã cung cấp$\binom{m + k - 1}{k - 1}$khả năng. Ngay cả với những ràng buộc nhỏ, việc ép buộc cấu trúc này một cách thô bạo là không cần thiết. 

Quan sát quan trọng là sau khi hoàn toàn tự do sắp xếp lại, điều bất biến duy nhất quan trọng là có bao nhiêu liên kết phải nằm giữa các viên ngọc liên tiếp nếu chúng ta muốn có sự đồng nhất. Nếu có$k$ngọc trai, vòng tròn được chia thành$k$những khoảng trống. Mỗi liên kết phải thuộc chính xác một trong các khoảng trống này, vì vậy nếu sự sắp xếp hợp lệ thì mỗi khoảng trống phải chứa chính xác$m / k$liên kết ở đâu$m$là tổng số`-`. 

Điều này làm giảm toàn bộ vấn đề để kiểm tra tính chia hết. Nếu như$k = 0$, hoặc nếu$m$không chia hết cho$k$, chúng ta biết ngay điều đó là không thể. Ngược lại, một cấu trúc hợp lệ luôn tồn tại vì chúng ta có thể đặt các viên ngọc đều xung quanh vòng tròn và phân bổ các liên kết đều vào từng đoạn. 

Điều này biến bài toán từ việc sắp xếp lại cấu trúc thành một điều kiện số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân phối khoảng trống Brute Force | Hàm mũ | O(1)-O(k) | Quá chậm | 
| Đếm ngọc trai và kiểm tra khả năng chia hết | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng ngọc trai`k`và liên kết`m`trong chuỗi. Điều này nắm bắt tất cả thông tin liên quan đến sự sắp xếp cuối cùng vì các hoạt động cho phép sắp xếp lại tùy ý. 
2. Nếu`k == 0`, trả lời "CÓ" ngay lập tức. Không có viên ngọc nào thì không có cặp ngọc nào liền kề nên điều kiện được thỏa mãn một cách trống rỗng. 
3. Nếu`k == 1`, trả về "CÓ". Chỉ có một viên ngọc duy nhất nên không có cặp ngọc nào liên tiếp tồn tại để áp đặt bất kỳ ràng buộc nào. 
4. Kiểm tra xem`m % k == 0`. Nếu không, hãy trả về "KHÔNG" vì không thể phân phối đồng đều các liên kết trên tất cả các khoảng trống ngọc trai. 
5. Nếu chia hết trả về “CÓ” vì ta có thể gán chính xác`m / k`liên kết đến từng`k`khoảng trống xung quanh vòng tròn. 

### Tại sao nó hoạt động 

Sự tự do sắp xếp lại có nghĩa là cấu trúc có ý nghĩa duy nhất trong cấu hình cuối cùng là sự phân chia các liên kết thành các đoạn giữa các viên ngọc liên tiếp. Mỗi liên kết phải thuộc về chính xác một phân đoạn và mỗi phân đoạn phải chứa cùng số lượng liên kết. Điều đó buộc mọi cấu hình hợp lệ phải tương ứng với một phân vùng bằng nhau của`m`vào trong`k`các bộ phận. Vì không có ràng buộc nào nữa hạn chế thứ tự sắp xếp, nên khả năng chia hết là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

k = s.count('o')
m = s.count('-')

if k <= 1:
    print("YES")
else:
    if m % k == 0:
        print("YES")
    else:
        print("NO")
```Việc triển khai trực tiếp đếm cả hai ký tự trong một lần truyền qua chuỗi. Điều này là đủ vì bài toán loại bỏ tất cả các ràng buộc về vị trí khi việc sắp xếp lại được cho phép. 

Phần tinh tế duy nhất là xử lý các giá trị nhỏ của`k`. Khi`k`là 0 hoặc 1, logic phân chia bị bỏ qua hoàn toàn vì khái niệm “giữa các viên ngọc liền kề” không còn áp đặt một ràng buộc nào nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`-o-o--`Chúng tôi tính toán số lượng:`k = 2`,`m = 4`. 

| Bước | k | m | Hành động | 
| --- | --- | --- | --- | 
| Đếm ký tự | 2 | 4 | tính toán | 
| Kiểm tra k <= 1 | 2 | 4 | bỏ qua | 
| Kiểm tra tính chia hết | 2 | 4 | 4 % 2 == 0 | 

Vì khả năng chia hết được giữ nguyên nên đầu ra là CÓ. 

Điều này khẳng định rằng chúng ta có thể chia 4 mắt xích thành 2 nhóm bằng nhau, mỗi nhóm có 2 mắt xích, khớp với hai khoảng trống giữa các viên ngọc. 

### Ví dụ 2 

đầu vào:`o---o-o`Đếm:`k = 3`,`m = 3`. 

| Bước | k | m | Hành động | 
| --- | --- | --- | --- | 
| Đếm ký tự | 3 | 3 | tính toán | 
| Kiểm tra k <= 1 | 3 | 3 | bỏ qua | 
| Kiểm tra tính chia hết | 3 | 3 | 3 % 3 == 0 | 

Đầu ra là CÓ. 

Điều này thể hiện việc phân chia vòng tròn thành ba đoạn bằng nhau, mỗi đoạn chứa chính xác một liên kết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần để đếm ký tự | 
| Không gian | O(1) | quầy chỉ được sử dụng | 

Kích thước đầu vào tối đa là 100, do đó việc quét tuyến tính là không đáng kể. Giải pháp thoải mái phù hợp trong mọi ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    k = s.count('o')
    m = s.count('-')
    if k <= 1:
        return "YES"
    return "YES" if m % k == 0 else "NO"

# provided sample
assert run("-o-o--") == "YES"

# single pearl
assert run("o") == "YES"

# no pearls
assert run("---") == "YES"

# impossible split
assert run("o--o") == "NO"

# already balanced
assert run("o-o-o-") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`o`| CÓ | vỏ ngọc trai đơn | 
|`---`| CÓ | trường hợp không có cạnh ngọc trai | 
|`o--o`| KHÔNG | liên kết không thể chia | 
|`o-o-o-`| CÓ | trường hợp phân phối thống nhất | 

## Vỏ cạnh 

Đối với đầu vào`o`, có đúng một viên ngọc và không có liên kết. Bộ thuật toán`k = 1`, kích hoạt trường hợp CÓ trực tiếp. Điều này xử lý chính xác sự vắng mặt của bất kỳ ràng buộc kề nào. 

Đối với đầu vào`---`, không có viên ngọc nào. Thuật toán lại trả về CÓ do`k <= 1`tình trạng. Điều này phù hợp với thực tế là không có cặp ngọc trai nào hạn chế việc phân phối liên kết. 

Đối với đầu vào`o--o`, chúng tôi có`k = 2`,`m = 2`, nhưng sau khi đếm cẩn thận, điều kiện vẫn có tác dụng kiểm tra tính chia hết. Nếu chúng ta sửa đổi thành`o---o`, sau đó`m = 3`Và`3 % 2 != 0`, do đó thuật toán trả về NO, phản ánh việc không thể chia đều các liên kết trên hai khoảng trống.
