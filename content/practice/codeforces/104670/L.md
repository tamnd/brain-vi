---
title: "CF 104670L - Châu chấu"
description: "Mỗi dòng đầu vào mô tả một cặp sự kiện định kỳ. Đối với một cặp nhất định, hai loài xuất hiện trở lại sau mỗi số năm cố định và chúng ta được biết vào năm cuối cùng khi cả hai loài xuất hiện cùng nhau. Từ thông tin đó, chúng tôi muốn dự đoán khi nào cặp đôi đó sẽ xuất hiện cùng nhau."
date: "2026-06-29T09:37:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "L"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 43
verified: true
draft: false
---

[CF 104670L - Locust Locus](https://codeforces.com/problemset/problem/104670/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả một cặp sự kiện định kỳ. Đối với một cặp nhất định, hai loài xuất hiện trở lại sau mỗi số năm cố định và chúng ta được biết vào năm cuối cùng khi cả hai loài xuất hiện cùng nhau. Từ thông tin đó, chúng tôi muốn dự đoán khi nào cặp đôi đó sẽ xuất hiện cùng nhau. 

Vì vậy, đối với mỗi bộ ba bao gồm một năm đồng thời cuối cùng và hai độ dài chu kỳ, chúng ta đang xem xét một cách hiệu quả mô hình lặp lại trên trục số: bắt đầu từ năm đó, một sự kiện lặp lại mỗi năm.`c1`năm và năm khác mỗi`c2`năm. Cặp này trùng khớp bất cứ khi nào cả hai chu kỳ thẳng hàng trở lại. 

Nhiệm vụ là tính toán năm trùng hợp tiếp theo cho mỗi cặp và sau đó chọn ra cặp sớm nhất trong số tất cả các cặp. 

Các ràng buộc đủ nhỏ để chúng ta có thể tính toán trực tiếp câu trả lời cho mỗi cặp mà không cần bất kỳ thủ thuật tối ưu hóa nào ngoài số học cơ bản. Với tối đa 99 cặp và độ dài chu kỳ dưới 100, ngay cả việc tính toán trực tiếp trên mỗi cặp cũng không đáng kể trong thời gian không đổi. 

Một sai lầm ngây thơ có thể xuất hiện ở đây là cho rằng lần xuất hiện đồng thời tiếp theo chỉ đơn giản là`y + max(c1, c2)`. Điều đó không thành công vì việc căn chỉnh phụ thuộc vào cả hai chu kỳ cùng một lúc, không chỉ chu kỳ chậm hơn. 

Ví dụ, nếu`y = 2000`,`c1 = 6`,`c2 = 4`, sau đó cộng 6 sẽ có 2006, nhưng 2006 không chia hết cho 4 từ căn chỉnh cơ sở, vì vậy nó không phải là sự xuất hiện đồng thời hợp lệ. Sự trùng hợp chính xác tiếp theo được xác định bởi bội số chung nhỏ nhất. 

Một vấn đề tế nhị khác là quên rằng chúng ta được yêu cầu _năm đầu tiên trong số tất cả các cặp_, chứ không phải cặp đầu tiên một cách độc lập. Mỗi cặp tạo ra một năm ứng cử viên và chỉ sau khi tính toán tất cả các ứng cử viên, chúng tôi mới so sánh chúng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là mô phỏng từng năm bắt đầu từ`y + 1`, kiểm tra từng năm ứng viên xem nó có phù hợp với cả hai chu kỳ hay không. Đối với một cặp nhất định, điều này có nghĩa là liên tục kiểm tra xem`(year - y) % c1 == 0`Và`(year - y) % c2 == 0`. Vì chu kỳ tối đa là 99 nên trận đấu tiếp theo được đảm bảo trong vòng tối đa`lcm(c1, c2)`các bước được giới hạn bởi 9801. Với tối đa 99 cặp, cách tiếp cận này vẫn có thể chấp nhận được nhưng nó mang tính gián tiếp không cần thiết. 

Một quan sát rõ ràng hơn xuất phát từ việc hiểu “cả hai chu kỳ đều sắp xếp lại” nghĩa là gì về mặt cấu trúc. Khi hai chuỗi định kỳ trùng nhau vào năm`y`, chúng sẽ lại trùng nhau một cách chính xác mọi`lcm(c1, c2)`năm. Điều này loại bỏ bất kỳ nhu cầu mô phỏng. Lần xuất hiện tiếp theo được cố định là`y + lcm(c1, c2)`. 

Điều này làm giảm nhiệm vụ tính toán một giá trị số học cho mỗi cặp và lấy giá trị tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k · lcm(c1,c2)) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tính toán trực tiếp LCM | O(k log min(c1,c2)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi cặp, đọc đồng thời năm cuối cùng`y`và độ dài chu kỳ`c1`,`c2`. Điều này xác định vấn đề căn chỉnh lặp lại được neo tại`y`. 
2. Tính ước chung lớn nhất của`c1`Và`c2`, sau đó rút ra bội số chung nhỏ nhất bằng cách sử dụng`lcm = (c1 // gcd) * c2`. Điều này đưa ra khoảng thời gian chính xác sau đó cả hai chu kỳ đều căn chỉnh lại. 
3. Tính năm xuất hiện tiếp theo của cặp này là`y + lcm`. 
4. Theo dõi mức tối thiểu của tất cả các năm xuất hiện tiếp theo được tính toán trên tất cả các cặp, vì bài toán yêu cầu sự trùng hợp sớm nhất trong tương lai giữa tất cả các cặp loài. 

### Tại sao nó hoạt động 

Khi hai quá trình tuần hoàn sắp xếp thẳng hàng tại một thời điểm, sự sắp xếp trong tương lai của chúng tạo thành một chuỗi tuần hoàn chặt chẽ được điều chỉnh bởi bội số chung nhỏ nhất của các chu kỳ riêng lẻ của chúng. Bất kỳ sự trùng hợp nào sớm hơn`lcm(c1, c2)`sẽ mâu thuẫn với định nghĩa về bội số chung nhỏ nhất, vì nó sẽ bao hàm một khoảng thời gian chia sẻ nhỏ hơn. Do đó, mỗi cặp đóng góp chính xác một ứng cử viên vào năm tới và việc chọn mức tối thiểu giữa các cặp sẽ duy trì tính chính xác trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    k = int(input())
    ans = 10**30

    for _ in range(k):
        y, c1, c2 = map(int, input().split())
        g = math.gcd(c1, c2)
        lcm = (c1 // g) * c2
        ans = min(ans, y + lcm)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng bộ ba và ngay lập tức chuyển nó thành một năm sự kiện xác định trong tương lai. Chi tiết triển khai chính là tính toán bội số chung nhỏ nhất bằng cách sử dụng phép chia an toàn số nguyên trước khi nhân, giúp ngăn ngừa tràn và duy trì tính chính xác ngay cả khi các giá trị gần với giới hạn trên. 

Đáp án cuối cùng được duy trì tăng dần, tránh việc lưu trữ tất cả các ứng viên. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
2
1992 13 17
1992 14 18
```Đối với cặp đầu tiên, chúng tôi tính toán`lcm(13, 17) = 221`, vậy năm tiếp theo là`1992 + 221 = 2213`. 

Đối với cặp thứ hai,`lcm(14, 18) = 126`, vậy năm tiếp theo là`1992 + 126 = 2118`. 

| Cặp | y | c1 | c2 | gcd | lcm | Năm tới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1992 | 13 | 17 | 1 | 221 | 2213 | 
| 2 | 1992 | 14 | 18 | 2 | 126 | 2118 | 

Câu trả lời là`2118`bởi vì nó nhỏ hơn trong hai năm ứng cử viên. Dấu vết này cho thấy mỗi cặp độc lập và chỉ đóng góp một sự kiện cố định trong tương lai. 

Bây giờ hãy xem xét:```
1
2001 5 7
```Đây`gcd(5,7)=1`, Vì thế`lcm=35`và lần xuất hiện tiếp theo là`2036`. Không có cặp cạnh tranh nên đây là câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log C) | Mỗi cặp yêu cầu tính toán gcd trên các giá trị lên tới 99, giá trị này được giới hạn không đổi trong thực tế | 
| Không gian | O(1) | Chỉ một số số nguyên được duy trì bất kể kích thước đầu vào | 

Các ràng buộc cho phép tối đa 99 cặp và mỗi phép tính là số học theo thời gian không đổi, do đó giải pháp có thể chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import gcd

    k = int(input())
    ans = 10**30
    for _ in range(k):
        y, c1, c2 = map(int, input().split())
        g = gcd(c1, c2)
        lcm = (c1 // g) * c2
        ans = min(ans, y + lcm)
    return str(ans)

def run(inp: str) -> str:
    return solve_io(inp)

# provided samples (from statement image, reconstructed format)
assert run("2\n1992 13 17\n1992 14 18\n") == "2118"
assert run("1\n2001 5 7\n") == "2036"

# custom cases
assert run("1\n2020 2 3\n") == "2026", "minimum case style"
assert run("3\n2010 2 3\n2011 4 6\n2012 5 7\n") == str(min(2010+6, 2011+12, 2012+35)), "multiple pairs"
assert run("1\n2021 99 99\n") == str(2021 + 99), "equal cycles"
assert run("2\n1800 1 1\n1801 2 3\n") == str(min(1800+1, 1801+6)), "boundary gcd"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cặp chu kỳ nhỏ | 2026 | hành vi lcm nhỏ nhất có ý nghĩa | 
| nhiều cặp | tính toán tối thiểu | lựa chọn tối thiểu toàn cầu | 
| chu kỳ bằng nhau | y + c | trường hợp cạnh gcd trong đó lcm giảm | 
| trường hợp gcd ranh giới | đúng phút | sự đúng đắn ở mức cực đoan | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên xảy ra khi cả hai độ dài chu kỳ đều giống nhau. Đối với một đầu vào như`2021 10 10`, gcd bằng chính số đó, do đó lcm giảm xuống 10. Thuật toán tạo ra một cách chính xác`2021 + 10`, và không cần xử lý đặc biệt vì công thức xử lý trường hợp này một cách tự nhiên. 

Một trường hợp khác là khi một chu kỳ chia cho chu kỳ kia, chẳng hạn như`2010 4 12`. Ở đây gcd là 4, vì vậy lcm trở thành 12 chứ không phải 16. Cách nhân đơn giản mà không chia cho gcd sẽ đánh giá quá cao khoảng thời gian và tạo ra một năm sau không chính xác, trong khi công thức được triển khai tránh tính hai lần các hệ số chung. 

Trường hợp đặc biệt cuối cùng là khi nhiều cặp tạo ra các năm ứng cử viên rất gần nhau. Vì chúng tôi chỉ theo dõi hoạt động tối thiểu nên thuật toán xử lý chính xác các mối quan hệ và thứ tự mà không yêu cầu sắp xếp hoặc lưu trữ các kết quả trung gian.
