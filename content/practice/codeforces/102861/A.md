---
title: "CF 102861A - Album Nhãn Dán"
description: "Album có N ô trống và mỗi gói đóng góp một số lượng nhãn dán ngẫu nhiên. Một gói có thể chứa bất kỳ số nguyên nào giữa A và B, với mọi giá trị đều có cùng xác suất."
date: "2026-07-25T14:00:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "A"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 45
verified: true
draft: false
---

[CF 102861A - Album hình dán](https://codeforces.com/problemset/problem/102861/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Album có`N`các khe trống và mỗi gói đóng góp một số lượng nhãn dán ngẫu nhiên. Một gói có thể chứa bất kỳ số nguyên nào giữa`A`Và`B`, với mọi giá trị đều có cùng xác suất. Mục tiêu không phải là mô phỏng một quy trình thu thập mà là để tính toán số lượng gói trung bình cần thiết cho đến khi số nhãn tích lũy đạt ít nhất`N`. 

Các giá trị đầu vào mô tả số lượng nhãn dán mục tiêu cũng như kích thước gói nhỏ nhất và lớn nhất có thể. Đầu ra là một giá trị dấu phẩy động biểu thị số lượng gói yêu cầu dự kiến. 

Ràng buộc`N <= 10^6`là khó khăn chính. Giải pháp thử mọi lịch sử bộ sưu tập có thể là không thể vì số lượng lịch sử tăng theo cấp số nhân. Ngay cả một giải pháp lập trình động thực hiện chuyển đổi trên mọi kích thước gói có thể có cho mọi trạng thái cũng sẽ cần khoảng`N * (B - A + 1)`các hoạt động có thể đạt tới`10^12`khi cả hai giá trị đều lớn. Giải pháp phải xử lý từng trạng thái trong thời gian gần như không đổi. 

Có một số trường hợp công thức trực tiếp có thể thất bại. Đầu tiên là khi các gói có thể không chứa nhãn dán. Ví dụ, với đầu vào`1 0 1`, một gói sẽ có 0 hoặc 1 nhãn dán. Câu trả lời dự kiến ​​là`2.0`, bởi vì gói đầu tiên thành công với xác suất một nửa và số lần thử tuân theo phân bố hình học. Công thức chia cho kích thước gói trung bình sẽ không thành công vì giá trị trung bình không đủ thông tin khi có thể tiến triển bằng 0. 

Một trường hợp khác là khi mọi gói có cùng kích thước. Đối với đầu vào`30 3 3`, mỗi gói có chính xác ba nhãn dán, vì vậy câu trả lời là`10.0`. Việc xử lý phạm vi như thể nó chứa nhiều giá trị có khả năng như nhau sẽ tạo ra sự phân chia không chính xác cho độ dài phạm vi. 

Trường hợp ranh giới cuối cùng xuất hiện khi kích thước gói tối thiểu đã kết thúc ở trạng thái nhỏ. Đối với đầu vào`5 5 10`, một gói luôn hoàn thành album, vì vậy câu trả lời là`1.0`. Một sự lặp lại vô tình bao gồm các trạng thái trước đó không thể xảy ra có thể tạo ra các giá trị lớn hơn một. 

## Phương pháp tiếp cận 

Một giải pháp lập trình động đơn giản xác định`dp[x]`như số lượng gói tin cần thiết khi`x`nhãn dán vẫn còn thiếu. Nếu một gói chứa`k`nhãn dán, số tiền còn lại sẽ trở thành`max(0, x-k)`. Sự tái phát là`dp[x] = 1 + average(dp[max(0, x-k)])`trên tất cả các kích cỡ gói có thể. 

Cách tiếp cận này đúng vì mọi trạng thái chỉ phụ thuộc vào lượng còn lại nhỏ hơn. Vấn đề là sự chuyển đổi. Đối với mọi`x`, kiểm tra mọi giá trị từ`A`ĐẾN`B`có thể yêu cầu tới một triệu hoạt động. Với một triệu tiểu bang, điều này trở thành khoảng`10^12`trong trường hợp xấu nhất vượt xa giới hạn. 

Quan sát quan trọng là quá trình chuyển đổi sử dụng một loạt các trạng thái trước đó liên tiếp. Khi`A > 0`, các giá trị`dp[x-B]`bởi vì`dp[x-A]`là những đóng góp khác không duy nhất. Khi`A = 0`, quá trình chuyển đổi cũng chứa`dp[x]`chính nó vì gói nhãn dán bằng 0 sẽ giữ trạng thái không thay đổi. Trong trường hợp đó, phép truy toán có thể được sắp xếp lại theo đại số để giá trị hiện tại vẫn được tính theo thời gian không đổi. 

Việc duy trì tổng tiền tố của các giá trị lập trình động cho phép chúng tôi truy xuất bất kỳ tổng phạm vi liên tiếp nào trong thời gian không đổi. Quá trình chuyển đổi bậc hai ban đầu trở thành quá trình quét tuyến tính từ`1`ĐẾN`N`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N(B - A + 1)) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp`Ở đâu`dp[x]`lưu trữ số lượng gói dự kiến ​​​​cần thiết để thu thập`x`nhiều nhãn dán hơn. Cũng duy trì`pref[x]`, tổng tiền tố của tất cả`dp`giá trị lên đến`x`. 
2. Xử lý các trạng thái từ`1`ĐẾN`N`. Trước tiên, chúng tôi luôn giải quyết số tiền còn thiếu nhỏ hơn, điều đó có nghĩa là mọi giá trị cần thiết cho phép truy toán đều đã được biết. 
3. Nếu`A > 0`, xử lý tình trạng tái phát bình thường. Các trạng thái trước đó có thể hình thành khoảng cách từ`max(0, x-B)`ĐẾN`x-A`. Sử dụng tổng tiền tố để lấy tổng của khoảng này và tính:`dp[x] = 1 + interval_sum / (B-A+1)`các`1`biểu thị việc mua gói tiếp theo và mức trung bình biểu thị công việc còn lại dự kiến ​​sau gói đó. 
4. Nếu`A = 0`, cô lập trạng thái hiện tại khỏi sự tái diễn. Gói nhãn dán không đóng góp`dp[x]`chính nó nên phương trình trở thành:`dp[x] = (B+1 + sum(dp[j]) for j in [max(0,x-B), x-1]) / B`Điều này tránh sự phụ thuộc vòng tròn và cho phép lặp lại tương tự. 
5. Sau khi tính toán từng`dp[x]`, hãy cập nhật tổng tiền tố của nó để các trạng thái trong tương lai có thể truy vấn phạm vi ngay lập tức. 

Tại sao nó hoạt động: Trạng thái lập trình động thể hiện chính xác số lượng gói còn lại dự kiến ​​cho mỗi số lượng nhãn dán bị thiếu có thể có. Sự tái phát diễn ra trực tiếp từ việc điều hòa gói đầu tiên được mua. Tổng tiền tố chỉ thay đổi cách tính giá trị trung bình cần thiết chứ không thay đổi giá trị toán học của phép truy hồi. Việc xử lý đặc biệt của`A = 0`loại bỏ trường hợp duy nhất trong đó trạng thái hiện tại phụ thuộc vào chính nó, vì vậy mọi trạng thái đều được tính toán từ các giá trị cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, A, B = map(int, input().split())

    dp = [0.0] * (N + 1)
    pref = [0.0] * (N + 1)

    count = B - A + 1

    for x in range(1, N + 1):
        if A == 0:
            left = max(0, x - B)
            if left == 0:
                previous_sum = pref[x - 1]
            else:
                previous_sum = pref[x - 1] - pref[left - 1]
            dp[x] = (B + 1 + previous_sum) / B
        else:
            left = max(0, x - B)
            right = x - A
            if right >= left:
                interval_sum = pref[right]
                if left > 0:
                    interval_sum -= pref[left - 1]
            else:
                interval_sum = 0.0
            dp[x] = 1.0 + interval_sum / count

        pref[x] = pref[x - 1] + dp[x]

    print("{:.10f}".format(dp[N]))

if __name__ == "__main__":
    solve()
```Mảng`dp`lưu trữ câu trả lời cho mọi mục tiêu nhỏ hơn, trong khi`pref`lưu trữ tổng tích lũy để có thể truy xuất một loạt trạng thái mà không cần lặp. Cập nhật tổng tiền tố xảy ra ngay sau khi tính toán một trạng thái vì các trạng thái sau này phụ thuộc vào tất cả các giá trị trước đó. 

Chi nhánh cho`A == 0`là cần thiết vì phép truy hồi thông thường chứa giá trị chưa biết`dp[x]`. Sau khi chuyển số hạng đó sang bên trái, mẫu số sẽ trở thành`B`, không`B+1`. Thiếu điều chỉnh đại số này là lỗi triển khai phổ biến nhất. 

Vì`A > 0`,`right = x - A`là trạng thái trước đó lớn nhất có thể còn lại sau khi nhận được gói nhỏ nhất. Việc sử dụng giới hạn dưới`max(0, x-B)`bởi vì nhận được một gói lớn có thể hoàn thành album, tương ứng với`dp[0] = 0`. 

Các giá trị dấu phẩy động của Python là đủ vì dung sai lỗi yêu cầu là`10^-5`. 

## Ví dụ đã hoạt động 

Đối với đầu vào`40 0 2`, trường hợp gói 0 được sử dụng. 

| x | Số tiền trước đó đã sử dụng | dp[x] | 
| --- | --- | --- | 
| 1 | dp[0] = 0 | 1,50000 | 
| 2 | dp[0] + dp[1] = 1,50000 | 2,25000 | 
| 3 | dp[1] + dp[2] = 3,75000 | 3.37500 | 
| 40 | dp[38] + dp[39] | 40.33333 | 

Dấu vết cho thấy các gói nhãn dán bằng 0 làm tăng kỳ vọng như thế nào. Việc lặp lại không thể đơn giản sử dụng kích thước gói trung bình vì các gói bị lỗi vẫn tiêu tốn nhiều lần thử. 

Đối với đầu vào`100 1 10`, mọi gói đều có tiến triển tích cực, do đó quá trình chuyển đổi bình thường được sử dụng. 

| x | Phạm vi của các trạng thái trước đó | dp[x] | 
| --- | --- | --- | 
| 1 | trống | 1,00000 | 
| 2 | dp[1] | 1.10000 | 
| 3 | dp[1] tới dp[2] | 1.21000 | 
| 100 | dp[90] tới dp[99] | 18.72727 | 

Dấu vết chứng tỏ rằng chỉ cần một khoảng trượt của các giá trị trước đó. Tổng tiền tố cho phép khoảng thời gian đó được truy vấn ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi số lượng nhãn dán được xử lý một lần và mỗi lần chuyển đổi đều sử dụng tổng tiền tố. | 
| Không gian | O(N) | Thuật toán lưu trữ một giá trị và một tổng tiền tố cho mỗi số nhãn dán còn lại có thể có. | 

Tối đa một triệu trạng thái là thực tế đối với thuật toán tuyến tính. Việc sử dụng bộ nhớ cũng tuyến tính và vừa vặn vì chỉ có hai mảng giá trị dấu phẩy động được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    N, A, B = map(int, sys.stdin.readline().split())

    dp = [0.0] * (N + 1)
    pref = [0.0] * (N + 1)

    for x in range(1, N + 1):
        if A == 0:
            left = max(0, x - B)
            s = pref[x - 1]
            if left > 0:
                s -= pref[left - 1]
            dp[x] = (B + 1 + s) / B
        else:
            left = max(0, x - B)
            right = x - A
            s = 0.0
            if right >= left:
                s = pref[right]
                if left > 0:
                    s -= pref[left - 1]
            dp[x] = 1 + s / (B - A + 1)
        pref[x] = pref[x - 1] + dp[x]

    sys.stdin = old_stdin
    return "{:.5f}".format(dp[N])

assert abs(float(solve_data("40 0 2")) - 40.33333) < 1e-5
assert abs(float(solve_data("100 1 10")) - 18.72727) < 1e-5
assert abs(float(solve_data("30 3 3")) - 10.0) < 1e-5
assert abs(float(solve_data("314 5 8")) - 48.74556) < 1e-5

assert abs(float(solve_data("1 1 1")) - 1.0) < 1e-5
assert abs(float(solve_data("1 0 1")) - 2.0) < 1e-5
assert abs(float(solve_data("5 5 10")) - 1.0) < 1e-5
assert abs(float(solve_data("1000000 1000000 1000000")) - 1.0) < 1e-5
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1.00000`| Mục tiêu tối thiểu với các gói xác định | 
|`1 0 1`|`2.00000`| Gói có kích thước bằng 0 và khả năng tái phát tự phụ thuộc | 
|`5 5 10`|`1.00000`| Gói tối thiểu đã hoàn thành album | 
|`1000000 1000000 1000000`|`1.00000`| Trường hợp ranh giới xác định kích thước tối đa | 

## Vỏ cạnh 

cho`1 0 1`, thuật toán đi vào nhánh tối thiểu bằng 0. Số tiền trước đó chỉ là`dp[0]`, bằng 0, vì vậy`dp[1] = (1 + 1 + 0) / 1 = 2`. Điều này khớp với số lần thử dự kiến ​​cho đến khi gói một nhãn dán đầu tiên thành công. 

Vì`30 3 3`, mỗi quá trình chuyển đổi chỉ có một kích thước gói có thể. Khoảng chứa chính xác trạng thái ba vị trí sau trạng thái hiện tại, do đó các giá trị trở thành`dp[3] = 1`,`dp[6] = 2`, và cuối cùng`dp[30] = 10`. Hành vi xác định được bảo tồn. 

Vì`5 5 10`, tất cả các kích cỡ gói đều đủ để hoàn thành ngay lập tức. Đối với mọi tiểu bang từ`1`ĐẾN`5`, phạm vi chuyển tiếp trống, để lại`dp[x] = 1`. Câu trả lời cuối cùng là đúng một gói. 

Vì`1000000 1000000 1000000`, thuật toán không cố gắng phân bổ bảng chuyển tiếp hoặc lặp qua các kích thước gói. Nó thực hiện một triệu cập nhật liên tục và trả về`1.0`, điều này chứng tỏ tại sao cách tiếp cận tuyến tính là cần thiết. 

Tôi cũng có thể điều chỉnh bài xã luận theo định dạng kiểu Codeforces ngắn hơn nếu bạn muốn một phiên bản gần giống với những gì sẽ xuất hiện trong bản phân tích cuộc thi chính thức.
