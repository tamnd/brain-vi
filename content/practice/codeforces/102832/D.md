---
title: "CF 102832D - Chuỗi vô nghĩa"
description: "Trình tự được xác định bằng phép truy toán liên quan đến bit AND. Số hạng ở vị trí n thu được bằng cách xem xét tất cả các vị trí trước đó có chỉ số được hình thành bằng cách xóa ít nhất một tập bit của n, lấy giá trị lớn nhất trong số các số hạng đó và nhân nó với c."
date: "2026-07-26T15:08:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102832
codeforces_index: "D"
codeforces_contest_name: "2020 China Collegiate Programming Contest Changchun Onsite"
rating: 0
weight: 102832
solve_time_s: 47
verified: true
draft: false
---

[CF 102832D - Trình tự vô nghĩa](https://codeforces.com/problemset/problem/102832/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Trình tự được xác định bằng phép truy toán liên quan đến bit AND. Thuật ngữ ở vị trí`n`thu được bằng cách xem xét tất cả các vị trí trước đó có chỉ số được hình thành bằng cách xóa ít nhất một tập bit của`n`, lấy giá trị lớn nhất trong số các số hạng đó và nhân nó với`c`. Nhiệm vụ là tính tổng của tất cả các số hạng từ chỉ số`0`thông qua chỉ số đã cho`n`, modulo`10^9+7`. Báo cáo vấn đề ban đầu và các ràng buộc là từ Codeforces Gym 102832D. 

Chỉ số đầu vào`n`không được đưa ra dưới dạng số nguyên thập phân thông thường. Nó được cung cấp dưới dạng chuỗi nhị phân và có thể chứa tới 3000 bit. Điều này ngay lập tức loại trừ mọi thứ phụ thuộc vào việc lặp qua mọi giá trị từ`0`ĐẾN`n`, bởi vì`n`bản thân nó có thể lớn về mặt thiên văn. Một số có khoảng 3000 chữ số nhị phân`2^3000`, do đó, ngay cả một thuật toán thực hiện một thao tác trên mỗi giá trị cũng không thể thực hiện được. Lời giải phải phụ thuộc vào số bit của`n`, không phải trên kích thước số của`n`. 

tham số`c`có thể lớn như`10^9`, nhưng tất cả các phép tính được thực hiện theo modulo`10^9+7`. Điều này có nghĩa là chúng ta chỉ cần phép nhân và lũy thừa mô-đun và chúng ta không bao giờ cần lưu trữ các giá trị chuỗi thực tế. 

Thử thách đầu tiên là hiểu được sự tái phát thực sự tạo ra điều gì. Việc triển khai trực tiếp sẽ cố gắng duy trì tất cả các giá trị trước đó, nhưng phạm vi chỉ mục khiến cách tiếp cận đó không thể sử dụng được. Các trường hợp hữu ích là những trường hợp tiết lộ mẫu ẩn. 

Đối với đầu vào`0 5`, số hạng duy nhất là`a_0 = 1`, vậy câu trả lời là`1`. Giải pháp xử lý chuỗi nhị phân như một số nguyên bình thường và cố gắng bắt đầu bit-DP từ bit được đặt đầu tiên có thể vô tình bỏ lỡ trường hợp 0. 

Đối với đầu vào`1 0`, các điều khoản là`a_0 = 1`Và`a_1 = 0`, vậy câu trả lời là`1`. Một công thức bất cẩn khi sử dụng`0^0`hoặc giả sử mọi thuật ngữ được tạo ra bởi sức mạnh của`c`không tách chỉ số 0 có thể tạo ra kết quả không chính xác. 

Đối với đầu vào`10 2`, các chỉ số là`0, 1, 2`, và giá trị của chúng là`1, 2, 2`, vậy câu trả lời là`5`. Một lỗi phổ biến là chỉ xử lý các bit đã đặt của`n`và quên đi sự đóng góp của tất cả các số nhỏ hơn xuất hiện trong phân tách nhị phân. 

# Phương pháp tiếp cận 

Giải pháp brute-force sẽ tạo ra chuỗi mỗi lần một thuật ngữ. Đối với mỗi chỉ số`x`, nó sẽ kiểm tra mọi chỉ số nhỏ hơn`i`, tính toán`x & i`và sử dụng giá trị tối đa đã được tính toán. Điều này đúng vì nó tuân theo sự tái diễn trực tiếp. Tuy nhiên, nó đòi hỏi phải xử lý tới`n`điều khoản và`n`có thể gần`2^{3000}`. Ngay cả khi bỏ qua việc tìm kiếm mức tối đa bên trong, việc chỉ truy cập mọi chỉ mục là không thể. 

Quan sát đầu tiên làm thay đổi vấn đề là`n & i`luôn là một mặt nạ con của`n`. Mọi chỉ mục có thể truy cập từ`n`bởi phép toán AND chỉ là một phiên bản của`n`với một số bit đã được loại bỏ. Nếu như`n`có`k`set bit, giá trị tốt nhất trước đó đến từ việc giữ càng nhiều bit set càng tốt trong khi loại bỏ ít nhất một trong số chúng. Điều này mang lại mối quan hệ:`a_n = c^(number of set bits in n)`. 

Trường hợp cơ bản cũng phù hợp vì`a_0 = 1 = c^0`. 

Bây giờ nhiệm vụ không còn là sự tái diễn kỳ lạ nữa. Chúng tôi chỉ cần:`sum(c^popcount(i))`với mọi số nhị phân`i`từ`0`ĐẾN`n`. 

Thử thách còn lại là tính toán số tiền này mà không liệt kê các con số. Chúng tôi xử lý các bit của`n`từ khía cạnh quan trọng nhất. Khi một chút`n`là`1`, tất cả các số có bit hiện tại được đặt thành`0`và các bit thấp còn lại tùy ý nhỏ hơn`n`. Sự đóng góp của họ có thể được tính là một khối hoàn chỉnh. Nếu có`k`các vị trí trống còn lại, khối đó chứa mọi lựa chọn có thể có của các bit đó, do đó tổng đóng góp của nó tuân theo một phép lặp đơn giản. 

Cho phép`f(k)`là tổng của`c^popcount(x)`tổng thể`k`- số bit. Mỗi bit là một trong hai`0`hoặc`1`. Lựa chọn`0`giữ mức đóng góp không thay đổi trong khi lựa chọn`1`nhân nó với`c`, Vì thế:`f(k) = f(k-1) * (1 + c)`với`f(0) = 1`. 

Trong khi quét`n`, chúng tôi duy trì bao nhiêu`1`bit đã xuất hiện. Bất cứ khi nào chúng ta gặp phải một`1`, chúng ta cộng phần đóng góp của các số khớp với tất cả các bit trước đó, đặt`0`ở đây và sử dụng bất kỳ bit nào còn lại. Những số đó có hệ số đóng góp cố định từ các bit được đặt trước đó và đóng góp hậu tố hoàn chỉnh từ các vị trí còn lại. 

Hai cách tiếp cận này khác nhau như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(n) | Quá chậm | 
| Tối ưu | O( | n | ) | 

#Hướng dẫn thuật toán 

1. Đọc biểu diễn nhị phân của`n`và giá trị của`c`. Tính toán trước sự đóng góp của mọi độ dài hậu tố có thể. Đối với một hậu tố với`k`bit, lưu trữ tổng của`c^popcount`trên tất cả các số có thể được hình thành bởi các bit đó. Sự tái phát là`dp[k] = dp[k-1] * (1+c)`. 
2. Xử lý vụ việc`n = 0`riêng. Chỉ có một thuật ngữ duy nhất`a_0`, giá trị của nó là`1`. 
3. Quét các bit của`n`từ trái sang phải. Giữ`ones`, số lượng bit đã đặt đã được xử lý và`ans`, câu trả lời tích lũy từ tất cả các tiền tố nhỏ hơn. 
4. Khi bit hiện tại là`0`, tiếp tục quét. Mọi số hợp lệ có tiền tố này cũng phải có`0`đây. 
5. Khi bit hiện tại là`1`, đếm tất cả các số sử dụng`0`ở vị trí này. Chúng đã nhỏ hơn rồi`n`và các bit còn lại của chúng có thể là bất cứ thứ gì. Đóng góp là số cách để chọn các bit được đặt trước đó nhân với tổng hậu tố được tính toán trước. 
6. Sau khi thêm khối đó, hãy bao gồm`n`chính nó bằng cách coi bit hiện tại của nó là`1`, làm tăng số lượng bit thiết lập được nhìn thấy cho đến nay. 
7. Sau khi tất cả các bit được xử lý, hãy cộng phần đóng góp của`n`chính nó và xuất kết quả theo modulo`10^9+7`. 

Tại sao nó hoạt động: sự lặp lại tạo ra một giá trị chỉ phụ thuộc vào số lượng bit được đặt trong chỉ mục. Việc quét nhị phân phân vùng tất cả các số từ`0`ĐẾN`n`thành các nhóm rời rạc dựa trên vị trí đầu tiên nơi chúng trở nên nhỏ hơn`n`. Mỗi số xuất hiện trong đúng một nhóm như vậy và đóng góp của mỗi nhóm được tính từ các lựa chọn độc lập của các bit còn lại. Số lượng được duy trì trước đó`1`các bit chiếm chính xác phần đóng góp tiền tố cố định, do đó tổng cuối cùng chứa mọi thuật ngữ bắt buộc chính xác một lần. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    n, c = input().split()
    c = int(c) % MOD

    if n == "0":
        print(1)
        return

    m = len(n)

    suf = [1] * (m + 1)
    mul = (1 + c) % MOD
    for i in range(1, m + 1):
        suf[i] = suf[i - 1] * mul % MOD

    ans = 0
    ones = 0

    for idx, ch in enumerate(n):
        if ch == '1':
            rem = m - idx - 1
            ans += pow(c, ones, MOD) * suf[rem] % MOD
            ans %= MOD
            ones += 1

    ans += pow(c, ones, MOD)
    ans %= MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng tính toán trước`suf`lưu trữ sự đóng góp đầy đủ của tất cả các hậu tố. Nếu một hậu tố có thêm một bit thì mọi khả năng cũ sẽ xuất hiện hai lần: một lần với bit đó bằng 0 và một lần với bit đó bằng một. Bản sao thứ hai đạt được hệ số`c`, mang lại số nhân`1 + c`. 

Vòng lặp chính thực hiện phân rã nhị phân được mô tả ở trên. Biến`ones`đại diện cho số lượng bit được đặt trong tiền tố đã được cố định. Khi bit hiện tại là`1`, thay đổi nó thành`0`làm cho số nhỏ hơn và các bit thấp hơn không bị hạn chế. Thuật ngữ`pow(c, ones, MOD)`xử lý tiền tố cố định, trong khi`suf[rem]`xử lý hậu tố miễn phí. 

Việc bổ sung cuối cùng là cần thiết vì quá trình quét chỉ đếm những số trở nên nhỏ hơn ở một mức độ nào đó. Số gốc`n`chính nó không được bao gồm cho đến cuối cùng. 

Số nguyên Python không tràn, nhưng tất cả các sản phẩm đều được giảm modulo`10^9+7`để giữ các giá trị nhỏ. Độ dài nhị phân có thể là 3000, do đó việc sử dụng lũy ​​thừa mô-đun cũng an toàn. 

# Ví dụ đã hoạt động 

cho`1000 3`, số nhị phân là tám. Giá trị của mỗi chỉ mục phụ thuộc vào số lượng bit được đặt của nó. 

| Bước | Đã xử lý bit | Những cái trước | Đã thêm đóng góp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 |`f(3)=4^3=64`| 64 | 
| 2 | 0 | 1 | không | 64 | 
| 3 | 0 | 1 | không | 64 | 
| 4 | 0 | 1 | không | 64 | 
| Kết thúc | chính số 8 | 1 |`3`| 67 | 

Kết quả là`67`, phù hợp với mẫu Dấu vết cho thấy thuật toán đếm tất cả các số bên dưới`8`là một khối khi bit đầu tiên thay đổi từ`1`ĐẾN`0`. 

Vì`10 2`, số nhị phân là hai. 

| Bước | Đã xử lý bit | Những cái trước | Đã thêm đóng góp | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | hậu tố có độ dài 1 cho`1+2=3`| 3 | 
| 2 | 0 | 1 | không | 3 | 
| Kết thúc | chính số 2 | 1 |`2`| 5 | 

Câu trả lời là`5`, tương ứng với các giá trị dãy`a_0=1`,`a_1=2`, Và`a_2=2`. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | n | 
| Không gian | O( | n | 

Độ dài đầu vào tối đa chỉ là 3000 bit, do đó việc quét tuyến tính dễ dàng nằm trong giới hạn. Thuật toán không bao giờ phụ thuộc vào giá trị số của`n`, đó là yêu cầu chính cho vấn đề này. 

# Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def solve_data(data):
    n, c = data.split()
    c = int(c) % MOD

    if n == "0":
        return "1"

    m = len(n)
    suf = [1] * (m + 1)
    for i in range(1, m + 1):
        suf[i] = suf[i - 1] * (1 + c) % MOD

    ans = 0
    ones = 0

    for i, ch in enumerate(n):
        if ch == '1':
            ans += pow(c, ones, MOD) * suf[m - i - 1] % MOD
            ans %= MOD
            ones += 1

    ans = (ans + pow(c, ones, MOD)) % MOD
    return str(ans)

assert solve_data("1000 3") == "67", "sample 1"
assert solve_data("0 5") == "1", "zero index"
assert solve_data("1 0") == "1", "zero multiplier"
assert solve_data("10 2") == "5", "small binary value"
assert solve_data("111 1") == "8", "all values equal to one"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 5`|`1`| Xử lý riêng thuật ngữ chuỗi cơ sở | 
|`1 0`|`1`| Tay cầm`c = 0`chính xác | 
|`10 2`|`5`| Kiểm tra phân rã nhị phân nhỏ | 
|`111 1`|`8`| Kiểm tra trường hợp mỗi thuật ngữ có giá trị một | 

# Vỏ cạnh 

cho`0 5`, thuật toán trả về ngay`1`. Điều này tránh coi tập bit trống là một quá trình chuyển đổi bình thường và bảo toàn chính xác định nghĩa đặc biệt của`a_0`. 

Vì`1 0`, công thức cho`a_0 = 1`Và`a_1 = 0^1 = 0`. Trong quá trình quét, khối nhỏ hơn sẽ góp phần`1`, và số hạng cuối cùng đóng góp`0`, đưa ra câu trả lời đúng`1`. 

Vì`10 2`, quá trình quét sẽ thấy tập hợp bit đầu tiên. Nó đếm các số có bit đó bị xóa, đó là`0`Và`1`, cho`1 + 2 = 3`. Giá trị cuối cùng`a_2 = 2`sau đó được thêm vào. Phân vùng bao gồm mọi chỉ mục chính xác một lần, tạo ra`5`. 

Đối với một chuỗi nhị phân rất lớn, chẳng hạn như một chuỗi gồm 3000 chuỗi, thuật toán không bao giờ cố gắng tạo chuỗi. Nó chỉ thực hiện một lượng công việc cố định trên mỗi bit, vì vậy phương pháp tương tự vẫn được áp dụng.
