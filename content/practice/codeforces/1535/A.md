---
title: "CF 1535A - Vòng loại công bằng"
description: "Chúng tôi có một giải đấu playoff nhỏ với đúng bốn người chơi. Hai người đầu tiên đối đầu, hai người cuối cùng đối đầu và những người chiến thắng gặp nhau trong trận chung kết. Mỗi người chơi có một giá trị kỹ năng riêng và trong bất kỳ trận đấu nào, người chơi có kỹ năng cao hơn luôn giành chiến thắng."
date: "2026-06-10T15:42:45+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1535
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 110 (Rated for Div. 2)"
rating: 800
weight: 1535
solve_time_s: 381
verified: false
draft: false
---

[CF 1535A - Vòng loại công bằng](https://codeforces.com/problemset/problem/1535/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 6 phút 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một giải đấu playoff nhỏ với đúng bốn người chơi. Hai người đầu tiên đối đầu, hai người cuối cùng đối đầu và những người chiến thắng gặp nhau trong trận chung kết. Mỗi người chơi có một giá trị kỹ năng riêng và trong bất kỳ trận đấu nào, người chơi có kỹ năng cao hơn luôn giành chiến thắng. Chúng tôi được yêu cầu xác định xem giải đấu có “công bằng” hay không, nghĩa là hai tay vợt mạnh nhất thực sự gặp nhau trong trận chung kết. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là bốn số nguyên thể hiện kỹ năng của người chơi. Kết quả đầu ra phải là "CÓ" nếu giải đấu diễn ra công bằng, nếu không thì "KHÔNG". Với tối đa 10.000 trường hợp thử nghiệm và giá trị kỹ năng được giới hạn ở mức 100, chúng tôi không thể cung cấp giải pháp mô phỏng tất cả các hoán vị, nhưng mỗi trường hợp đều đủ nhỏ để xử lý bằng so sánh trực tiếp. 

Một điểm tinh tế nảy sinh vì người chơi không được ghép ngẫu nhiên. Nếu hai tay vợt có kỹ năng cao nhất ở cùng một bảng đấu thì một người sẽ bị loại ở bán kết. Ví dụ, với kỹ năng`9, 8, 1, 2`sắp xếp như`[s1, s2, s3, s4]`, trận bán kết là`9 vs 8`Và`1 vs 2`. Mặc dù`9`Và`8`mạnh nhất, họ gặp nhau ở bán kết nên trận chung kết sẽ diễn ra`9 vs 2`, điều đó thật không công bằng. Cách tiếp cận bất cẩn chỉ nhìn vào hai số lớn nhất mà không xem xét vị trí của chúng sẽ dẫn đến đáp án sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi trận đấu: so sánh`s1`vs`s2`Và`s3`vs`s4`để tìm ra người thắng trận bán kết, sau đó so sánh những người chiến thắng đó cho trận chung kết. Điều này phù hợp với bốn người chơi nhưng trở nên dài dòng và máy móc, mặc dù nó khả thi vì kích thước đầu vào rất nhỏ. 

Cái nhìn sâu sắc quan trọng là vị trí. Chúng ta chỉ cần biết hai kỹ năng mạnh nhất và chúng thuộc phe nào. Trận bán kết đầu tiên được quyết định bởi hai kỹ năng đầu tiên`[s1, s2]`, và trận bán kết thứ hai bởi`[s3, s4]`. Chúng tôi kiểm tra mức tối đa trong mỗi trận bán kết: người chiến thắng trong trận bán kết đầu tiên là`max(s1, s2)`, thứ hai là`max(s3, s4)`. Nếu hai người chiến thắng ở trận bán kết này là hai kỹ năng đứng đầu tổng thể thì trận chung kết sẽ có hai người đứng đầu, tạo nên sự công bằng cho giải đấu. Nếu không, một tuyển thủ hàng đầu đã bị loại quá sớm. 

Điều này chuyển đổi vấn đề từ mô phỏng sang so sánh tối đa đơn giản. Nó vừa đúng vừa cực kỳ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 
| So sánh tối đa tối ưu | O(1) mỗi trường hợp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,`t`. 
2. Với mỗi test, đọc 4 số nguyên`s1, s2, s3, s4`. 
3. Xác định đội thắng trận bán kết thứ nhất bằng cách lấy`max(s1, s2)`. 
4. Xác định đội thắng trận bán kết thứ hai bằng cách lấy`max(s3, s4)`. 
5. Xác định tổng thể hai kỹ năng hàng đầu bằng cách lấy hai số lớn nhất trong số`[s1, s2, s3, s4]`. 
6. Kiểm tra xem người chiến thắng trận bán kết có phải là hai kỹ năng hàng đầu hay không. Nếu có, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tại sao nó hoạt động: Bằng cách chỉ so sánh những người chiến thắng trong trận bán kết với hai người đứng đầu toàn cầu, chúng tôi nắm bắt được liệu bảng đấu có cho phép cả hai tay vợt hàng đầu vào chung kết hay không. Thứ tự các trận đấu đảm bảo không có tuyển thủ nào khác có thể bất ngờ vào chung kết nên những kiểm tra này cũng đủ để xác định tính công bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = list(map(int, input().split()))
    first_winner = max(s[0], s[1])
    second_winner = max(s[2], s[3])
    top_two = sorted(s, reverse=True)[:2]
    if first_winner in top_two and second_winner in top_two:
        print("YES")
    else:
        print("NO")
```Chúng tôi đọc tất cả đầu vào với`sys.stdin.readline`cho tốc độ.`first_winner`Và`second_winner`tương ứng với đội thắng bán kết. Việc sắp xếp danh sách giảm dần và lấy hai phần tử đầu tiên sẽ mang lại cho bạn hai kỹ năng hàng đầu trên toàn cầu. Việc kiểm tra tư cách thành viên xác nhận cả hai người chiến thắng trong trận bán kết đều nằm trong số hai người đứng đầu. sử dụng`in`tránh những sai lầm khi giả định thứ tự hoặc chỉ số, điều này có thể thất bại nếu hai chỉ số trên cùng không`s1`Và`s3`. 

## Ví dụ đã hoạt động 

Trường hợp thử nghiệm:`3 7 9 5`| Biến | Giá trị | 
| --- | --- | 
| s | [3, 7, 9, 5] | 
| người chiến thắng đầu tiên | tối đa(3, 7) = 7 | 
| giây_winner | tối đa(9, 5) = 9 | 
| đầu_hai | được sắp xếp([3,7,9,5], Reverse=True)[:2] = [9,7] | 
| So sánh | 7 trong top_two? Đúng; 9 ở top_two? Có | 
| Đầu ra | CÓ | 

Trường hợp thử nghiệm:`4 5 6 9`| Biến | Giá trị | 
| --- | --- | 
| s | [4,5,6,9] | 
| người chiến thắng đầu tiên | tối đa(4,5) = 5 | 
| giây_winner | tối đa(6,9) = 9 | 
| đầu_hai | [9,6] | 
| So sánh | 5 ở top_two? KHÔNG; 9 ở top_two? Có | 
| Đầu ra | KHÔNG | 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác liệu cả hai tay vợt hàng đầu có lọt vào trận chung kết hay không dựa trên những người chiến thắng ở trận bán kết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng một số lượng thao tác không đổi và có`t`trường hợp thử nghiệm. | 
| Không gian | O(1) | Chúng tôi lưu trữ một số lượng biến cố định cho mỗi trường hợp thử nghiệm; không có bộ nhớ bổ sung nào tăng theo kích thước đầu vào. | 

Được cho`t <= 10^4`và mỗi thao tác đều đơn giản, giải pháp dễ dàng chạy trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution
    t = int(input())
    for _ in range(t):
        s = list(map(int, input().split()))
        first_winner = max(s[0], s[1])
        second_winner = max(s[2], s[3])
        top_two = sorted(s, reverse=True)[:2]
        if first_winner in top_two and second_winner in top_two:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("4\n3 7 9 5\n4 5 6 9\n5 3 8 1\n6 5 3 2\n") == "YES\nNO\nYES\nNO", "sample 1"

# custom cases
assert run("1\n1 2 3 4\n") == "YES", "top two on opposite sides"
assert run("1\n4 3 2 1\n") == "NO", "top two on same side"
assert run("1\n10 20 30 40\n") == "YES", "ascending order"
assert run("1\n40 30 20 10\n") == "YES", "descending order"
assert run("1\n1 100 50 2\n") == "NO", "strongest with weakest on same side"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 | CÓ | Hai người đứng đầu đối diện nhau lọt vào chung kết | 
| 4 3 2 1 | KHÔNG | Hai người đứng đầu cùng một trận bán kết | 
| 10 20 30 40 | CÓ | Xử lý đơn hàng tăng chính xác | 
| 40 30 20 10 | CÓ | Đơn hàng giảm được xử lý đúng cách | 
| 1 100 50 2 | KHÔNG | Mạnh nhất kết hợp với yếu nhất thất bại công bằng | 

## Vỏ cạnh 

Nếu hai kỹ năng hàng đầu được ghép đôi trong trận bán kết thứ nhất hoặc thứ hai, thuật toán sẽ phát hiện sự không công bằng vì`first_winner`hoặc`second_winner`sẽ không phù hợp với hai người đứng đầu. Ví dụ, với`[100, 1, 99, 2]`,`first_winner = 100`,`second_winner = 99`,`top_two = [100, 99]`. Cả hai đội thắng bán kết đều ở`top_two`, vì vậy kết quả đầu ra là "CÓ." Thay vào đó nếu chúng ta có`[100, 99, 1, 2]`,`first_winner = 100`,`second_winner = 2`,`top_two = [100, 99]`. Đây`second_winner`không nằm trong top hai và đầu ra là "KHÔNG." Thuật toán xử lý các trường hợp đặc biệt này bằng cách so sánh trực tiếp những người chiến thắng ở trận bán kết với hai người đứng đầu toàn cầu, điều này đảm bảo tính chính xác.
