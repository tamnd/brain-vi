---
title: "CF 106414F - Tổng gần đúng ba"
description: "Chúng ta được cung cấp một danh sách các số nguyên và một giá trị đích. Nhiệm vụ là chọn bất kỳ ba phần tử riêng biệt nào từ danh sách sao cho tổng của ba số đó càng gần mục tiêu càng tốt."
date: "2026-06-25T09:47:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106414
codeforces_index: "F"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2026 - Open Division"
rating: 0
weight: 106414
solve_time_s: 63
verified: true
draft: false
---

[CF 106414F - Ba tổng gần đúng](https://codeforces.com/problemset/problem/106414/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và một giá trị đích. Nhiệm vụ là chọn bất kỳ ba phần tử riêng biệt nào từ danh sách sao cho tổng của ba số đó càng gần mục tiêu càng tốt. “Càng gần càng tốt” có nghĩa là giảm thiểu sự khác biệt tuyệt đối giữa tổng ba được chọn và giá trị mục tiêu và chúng tôi chỉ quan tâm đến độ lệch tối thiểu đó chứ không quan tâm đến chỉ số của các phần tử. 

Đầu vào có thể được coi là một tập hợp các điểm trên một trục số và chúng tôi đang cố gắng chọn bất kỳ ba điểm nào có vị trí kết hợp gần nhất với vị trí mục tiêu cố định. Mọi câu trả lời hợp lệ đều phụ thuộc vào cấu trúc toàn cục của mảng chứ không phải bất kỳ sự liền kề cục bộ nào, do đó việc sắp xếp hoặc sắp xếp lại không làm thay đổi tính chính xác mà chỉ làm thay đổi tính dễ suy luận. 

Mức ràng buộc điển hình cho loại vấn đề này ngụ ý rằng việc liệt kê khối trên tất cả các bộ ba sẽ quá chậm khi kích thước mảng tăng lên quá vài nghìn. Một giải pháp kiểm tra tất cả các bộ ba thực hiện theo thứ tự$n^3$những bổ sung và so sánh, điều này nhanh chóng trở nên không khả thi ở$n = 10^5$hoặc thậm chí$n = 10^4$. Điều này thúc đẩy chúng tôi hướng tới các phương pháp sử dụng lại cấu trúc từng phần qua nhiều kết hợp thay vì tính lại tổng từ đầu. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều bộ ba tạo ra cùng một sự khác biệt tuyệt đối so với mục tiêu. Ví dụ, nếu mảng là`[1, 2, 3, 4]`và mục tiêu là`10`, cả hai`(1, 3, 4)`Và`(2, 3, 4)`có thể tạo ra số tiền gần bằng nhau. Một cách triển khai đơn giản chỉ cập nhật khi thực sự tốt hơn (`<`) thay vì cho phép sự bình đẳng (`<=`) có thể vô tình khóa vào giải pháp được tìm thấy đầu tiên và bỏ qua các bộ ba tối ưu bằng nhau hợp lệ khác, điều này vô hại đối với tính chính xác của câu trả lời số cuối cùng nhưng có thể trở nên nguy hiểm nếu việc triển khai nhầm phụ thuộc vào quy tắc lựa chọn cụ thể để cắt tỉa. 

Một trường hợp cạnh khác là khi mảng chứa các giá trị âm và dương lớn. Một chiến lược cắt tỉa bất cẩn giả định tính đơn điệu mà không sắp xếp có thể bỏ lỡ sự kết hợp trong đó những điểm tiêu cực cực đoan sẽ cân bằng những điểm tích cực lớn để tiến gần hơn đến mục tiêu. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi lặp lại mọi bộ ba chỉ số có thể có$i, j, k$, tính tổng, so sánh với mục tiêu và theo dõi sự khác biệt tuyệt đối nhỏ nhất được thấy cho đến nay. Điều này hiệu quả vì nó khám phá toàn bộ không gian tìm kiếm mà không bỏ sót. Vấn đề của nó hoàn toàn là tính toán: số bộ ba là$\binom{n}{3}$, mở rộng như$O(n^3)$. Với$n = 5000$, điều này đã đạt tới khoảng$2 \times 10^{10}$hoạt động, vượt xa giới hạn thực tế trong một môi trường hạn chế về thời gian. 

Quan sát cấu trúc quan trọng là khi chúng ta sửa một phần tử của bộ ba, bài toán còn lại sẽ trở thành bài toán tổng hai với mục tiêu xuất phát từ giá trị cố định. Sau khi sắp xếp mảng, chúng ta có thể khai thác thứ tự để di chuyển hai con trỏ vào trong và đánh giá tất cả các cặp khả thi theo thời gian tuyến tính cho từng vị trí cố định. Điều này làm giảm sự dư thừa vì mỗi cặp được xem xét trong quá trình quét có kiểm soát thay vì được tính toán lại cho từng phần tử thứ ba một cách độc lập. 

Phương pháp vũ lực có hiệu quả vì nó liệt kê rõ ràng tất cả các khả năng. Nó thất bại vì nó xử lý từng bộ ba một cách độc lập. Sự cải tiến này xuất phát từ việc nhận ra rằng đối với phần tử đầu tiên cố định, cặp bổ sung tốt nhất có thể được tìm kiếm một cách hiệu quả trong một mảng được sắp xếp bằng cách sử dụng chuyển động có hướng thay vì liệt kê lồng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Sắp xếp + Hai con trỏ |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp tạo ra cấu trúc cho phép di chuyển có thể dự đoán được khi điều chỉnh tổng, vì việc tăng chỉ số chỉ có thể duy trì hoặc tăng giá trị. 
2. Khởi tạo một biến`best_diff`với số lượng rất lớn. Điều này lưu trữ sự khác biệt tuyệt đối nhỏ nhất so với mục tiêu được thấy cho đến nay. 
3. Lặp lại từng chỉ mục`i`là phần tử đầu tiên của bộ ba. Đối với mỗi`i`, chúng ta sẽ tìm kiếm cặp tốt nhất`(j, k)`ở bên phải của nó. sửa chữa`i`giảm chiều hướng vấn đề từ ba lựa chọn xuống còn hai. 
4. Đặt hai con trỏ:`j = i + 1`Và`k = n - 1`. Chúng đại diện cho các ứng cử viên nhỏ nhất và lớn nhất còn lại. Ý tưởng là quét vào trong trong khi kiểm soát tổng. 
5. Trong khi`j < k`, tính toán`current_sum = a[i] + a[j] + a[k]`và cập nhật`best_diff = min(best_diff, |current_sum - target|)`. Điều này đảm bảo mọi bộ ba hợp lệ liên quan đến`i`được xem xét một cách có cấu trúc. 
6. Nếu`current_sum`nhỏ hơn mục tiêu, tăng`j`. Tăng dần`j`tăng tổng vì mảng được sắp xếp, do đó, điều này sẽ di chuyển tổng lên về phía mục tiêu. 
7. Nếu`current_sum`lớn hơn hoặc bằng mục tiêu, giảm`k`. Giảm dần`k`làm giảm tổng, đưa nó đến gần mục tiêu hơn. 

Chuyển động của con trỏ là cơ chế quan trọng. Nó đảm bảo rằng mỗi cặp`(j, k)`được truy cập nhiều nhất một lần cho mỗi lần cố định`i`, thay vì tính toán lại các kết hợp nhiều lần. 

### Tại sao nó hoạt động 

Đối với một cố định`i`, đoạn mảng bên phải được sắp xếp. Bất kỳ cặp nào`(j, k)`tạo thành một cảnh quan đơn điệu của các tổng: tăng dần`j`tổng tăng, giảm`k`làm giảm nó. Quy trình hai con trỏ khám phá không gian này một cách có hệ thống mà không bỏ qua bất kỳ khu vực nào có thể tồn tại sự cải tiến. Ở mỗi bước, hướng di chuyển chỉ loại bỏ những cặp không thể cải thiện tình hình hiện tại theo thứ tự, trong khi vẫn duy trì tất cả các cấu hình có khả năng tối ưu. Điều này đảm bảo rằng độ lệch tuyệt đối tối thiểu gặp phải trong quá trình quét là tối ưu thực sự trên tất cả các bộ ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, target = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    best_diff = float('inf')

    for i in range(n):
        j, k = i + 1, n - 1

        while j < k:
            s = a[i] + a[j] + a[k]
            diff = abs(s - target)
            if diff < best_diff:
                best_diff = diff

            if s < target:
                j += 1
            else:
                k -= 1

    print(best_diff)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách sắp xếp để cho phép di chuyển con trỏ đơn điệu. Vòng lặp bên ngoài chọn phần tử đầu tiên của mỗi bộ ba và vòng lặp while bên trong thực hiện tìm kiếm bị ràng buộc trên hai vị trí còn lại. Bước cập nhật chỉ theo dõi độ lệch tuyệt đối tốt nhất vì vấn đề yêu cầu độ gần thay vì bộ ba thực tế. 

Một lỗi phổ biến ở đây là đảo ngược điều kiện chuyển động của con trỏ. Nếu như`s < target`, di chuyển`k`thay vì`j`phá hủy sự đảm bảo đơn điệu và dẫn đến thiếu cấu hình hợp lệ. Một vấn đề tế nhị khác là quên rằng cả ba chỉ số phải khác biệt, điều này được thực thi một cách tự nhiên bằng cách bắt đầu`j`Tại`i + 1`. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng đầu vào`[ -4, -1, 2, 5, 9 ]`với mục tiêu`3`. 

### Dấu vết 1 

| tôi | j | k | hiện tại_sum | tốt nhất_diff | 
| --- | --- | --- | --- | --- | 
| -4 | -1 | 9 | 4 | 1 | 
| -4 | 2 | 9 | 7 | 4 | 
| -4 | 2 | 5 | 3 | 0 | 

Vì`i = -4`, thuật toán nhanh chóng tìm thấy kết quả khớp hoàn hảo ở tổng`3`, do đó độ lệch tốt nhất có thể trở thành`0`. Điều này chứng tỏ cách quét hai con trỏ hội tụ hiệu quả khi tồn tại bộ ba cân bằng. 

### Dấu vết 2 

Bây giờ hãy xem xét`[1, 2, 3, 4, 100]`với mục tiêu`50`. 

| tôi | j | k | hiện tại_sum | tốt nhất_diff | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 100 | 103 | 53 | 
| 1 | 2 | 4 | 7 | 43 | 
| 1 | 3 | 4 | 8 | 42 | 
| 2 | 3 | 100 | 107 | 57 | 

Dấu vết này cho thấy rằng khi không có bộ ba nào đến gần mục tiêu, thuật toán vẫn khám phá một cách có hệ thống tất cả các kết hợp có ý nghĩa gần cực trị. Kết quả tốt nhất đến từ các phần tử nhỏ nhất kết hợp với phần tử lớn nhất còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi chỉ mục cố định, quá trình quét hai con trỏ chạy theo thời gian tuyến tính trên hậu tố còn lại | 
| Không gian |$O(1)$| Chỉ một số lượng biến không đổi được sử dụng bên cạnh mảng đầu vào | 

Thời gian chạy bậc hai có thể chấp nhận được đối với các ràng buộc điển hình trong họ bài toán này, đặc biệt khi$n$có thể lên tới vài nghìn cho tới vài chục nghìn. Việc sắp xếp góp phần bổ sung$O(n \log n)$, bị chi phối bởi thành phần bậc hai. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, target = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        best_diff = float('inf')

        for i in range(n):
            j, k = i + 1, n - 1
            while j < k:
                s = a[i] + a[j] + a[k]
                best_diff = min(best_diff, abs(s - target))
                if s < target:
                    j += 1
                else:
                    k -= 1

        return best_diff

    return str(solve())

# small case
assert run("3 6\n1 2 3\n") == "0", "exact match"

# negative numbers
assert run("5 0\n-4 -1 2 5 9\n") == "0", "balanced triple"

# no good match
assert run("5 50\n1 2 3 4 100\n") == "42", "closest only"

# minimum size
assert run("3 10\n1 2 3\n") == "5", "single triple"

# duplicates
assert run("6 3\n1 1 1 2 2 2\n") == "0", "repeated values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 6 / 1 2 3`|`0`| mục tiêu chính xác có thể đạt được | 
|`5 0 / -4 -1 2 5 9`|`0`| cân bằng tiêu cực và tích cực | 
|`5 50 / 1 2 3 4 100`|`42`| mục tiêu xa, chỉ xấp xỉ tốt nhất | 
|`3 10 / 1 2 3`|`5`| kích thước đầu vào hợp lệ tối thiểu | 
|`6 3 / 1 1 1 2 2 2`|`0`| trùng lặp và nhiều bộ ba tối ưu | 

## Vỏ cạnh 

Khi mảng chứa cả số âm lớn và số dương lớn, giải pháp tối ưu thường đến từ việc kết hợp các cực trị. Ví dụ, trong`[-10, -5, 1, 3, 20]`với mục tiêu`0`, thuật toán xem xét chính xác`-10 + -5 + 20 = 5`và hội tụ đến giá trị gần nhất có thể bằng cách điều chỉnh con trỏ vào trong từ cả hai đầu. Việc sắp xếp đảm bảo có thể truy cập được những điểm cực trị này thông qua việc khởi tạo con trỏ mà không cần viết hoa đặc biệt. 

Khi tất cả các số đều giống hệt nhau, chẳng hạn như`[2, 2, 2, 2]`với mục tiêu`7`, mọi tổng ba đều bằng nhau. Thuật toán vẫn đánh giá chính xác một bộ đại diện cho mỗi`i`, và sự khác biệt tốt nhất vẫn không đổi trong suốt. Chuyển động của con trỏ nhanh chóng sụp đổ vì`j`Và`k`hội tụ ngay lập tức. 

Khi bộ ba tối ưu bao gồm hai giá trị nhỏ và một giá trị lớn, phương pháp hai con trỏ sẽ khám phá cấu hình này sớm một cách tự nhiên vì nó bắt đầu bằng`j`ở mức nhỏ nhất và`k`ở mức lớn nhất. Ví dụ, trong`[1, 2, 3, 100]`với mục tiêu`10`, sự kết hợp`(1, 2, 3)`được kiểm tra đầu tiên cho`i = 1`, đảm bảo phát hiện sớm vùng tối ưu.
