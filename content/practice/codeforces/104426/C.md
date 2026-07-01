---
title: "CF 104426C - Cài đặt sự cố SYPUCPC"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi phần, chúng tôi bắt đầu với một loạt các vấn đề khó khăn. Chúng tôi được phép xóa bất kỳ tập hợp con nào của các giá trị này, nhưng chúng tôi phải để lại ít nhất một phần tử."
date: "2026-06-30T19:03:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "C"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 77
verified: false
draft: false
---

[CF 104426C - Khắc phục sự cố SYPUCPC](https://codeforces.com/problemset/problem/104426/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi phần, chúng tôi bắt đầu với một loạt các vấn đề khó khăn. Chúng tôi được phép xóa bất kỳ tập hợp con nào của các giá trị này, nhưng chúng tôi phải để lại ít nhất một phần tử. Sau khi xóa, “điểm” của tập còn lại được xác định là trung bình số học của nó. 

Nhiệm vụ là chọn một tập con không rỗng có giá trị trung bình càng nhỏ càng tốt. 

Các ràng buộc ngay lập tức định hình không gian giải pháp. Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là 100000, do đó, bất kỳ phương pháp bậc hai nào cho mỗi trường hợp thử nghiệm hoặc thậm chí quét lại các tập hợp con nhiều lần sẽ không thành công. Chúng tôi cần một cái gì đó tuyến tính cho mỗi trường hợp thử nghiệm hoặc tốt hơn. 

Một vấn đề tế nhị là tập hợp con tối ưu không nhất thiết phải có tất cả các phần tử hoặc chỉ phần tử tối thiểu mà thôi. Ví dụ, nếu chúng ta có`[100, 1, 2]`, chỉ lấy`1`cho trung bình`1`, đó là tối ưu. Nhưng trong`[1, 100, 100]`, chỉ lấy`1`vẫn là tốt nhất, mặc dù hầu hết các phần tử đều lớn. Điều này gợi ý rằng các phần tử lớn không bao giờ giúp giảm mức trung bình, nhưng chúng ta cần phải biết chính xác lý do. 

Các trường hợp cạnh phát sinh xung quanh kích thước lựa chọn: 

Nếu tất cả các số đều bằng nhau thì bất kỳ tập hợp con nào cũng có mức trung bình như nhau, do đó việc loại bỏ các phần tử không có tác dụng gì. Một cách tiếp cận đơn giản vẫn có thể thử đánh giá tập hợp con phức tạp nhưng câu trả lời vẫn giữ nguyên giá trị đó. 

Một góc khác là khi có một phần tử nhỏ nhất duy nhất. Ngay cả khi đó, việc thêm bất kỳ phần tử lớn hơn nào cũng sẽ làm tăng mức trung bình một cách nghiêm ngặt, do đó tập hợp con tốt nhất vẫn là tập hợp đơn chứa phần tử tối thiểu. 

Rủi ro chính của việc suy luận sai là giả định rằng chúng ta có thể cần nhiều yếu tố để “cân bằng” mức trung bình. Trực giác đó là sai khi tính trung bình thuần túy mà không có ràng buộc. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ xem xét mọi tập hợp con không trống, tính tổng và kích thước của nó, đồng thời theo dõi mức trung bình tối thiểu. Điều này đúng nhưng không khả thi ngay lập tức. Số lượng tập hợp con là theo cấp số nhân, cụ thể$2^N - 1$, cái nào cho$N = 10^5$thậm chí không thể biểu diễn được chứ đừng nói đến việc lặp lại. 

Chúng ta cần hiểu mức trung bình hoạt động như thế nào khi thêm các phần tử. Giả sử chúng ta có một tập hợp được chọn với giá trị trung bình$x$. Nếu chúng ta thêm một phần tử mới$a$, mức trung bình mới trở thành$$\frac{S + a}{k + 1}$$Ở đâu$S/k = x$. Mức trung bình mới này hoàn toàn nhỏ hơn$x$chỉ nếu$a < x$. Điều này mang lại cái nhìn sâu sắc về cấu trúc: cải thiện mức trung bình đòi hỏi phải thêm các phần tử nhỏ hơn mức trung bình hiện tại. 

Bây giờ hãy xem xét bắt đầu từ bất kỳ tập hợp con hợp lệ nào. Nếu nó chứa nhiều hơn một phần tử thì trung bình của nó ít nhất là phần tử tối thiểu trong tập hợp con đó và việc bao gồm bất kỳ phần tử nào lớn hơn mức trung bình hiện tại sẽ không giúp ích gì. Điều này gợi ý một đối số thu hẹp tham lam: việc loại bỏ các phần tử không bao giờ tăng khả năng kiểm soát đối với mức trung bình tối thiểu và việc thêm các phần tử không thể đánh bại phần tử tối thiểu đã có trừ khi chúng thậm chí còn nhỏ hơn. 

Do đó, chiến lược tối ưu rút gọn thành một quan sát rất đơn giản: đạt được mức trung bình tốt nhất có thể bằng cách lấy một tập hợp con bao gồm một phần tử duy nhất và để giảm thiểu nó, chúng ta chọn phần tử nhỏ nhất trong mảng. 

Chúng tôi so sánh các phương pháp dưới đây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các tập hợp con | O(2^N) | O(N) | Quá chậm | 
| Kiểm tra tất cả các giá trị trung bình của tập hợp con thông qua sắp xếp hoặc DP | O(N^2) hoặc tệ hơn | O(N) | Quá chậm | 
| Lấy phần tử tối thiểu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc dãy độ khó. Chúng tôi xử lý từng thử nghiệm một cách độc lập vì việc xóa không tương tác giữa các trường hợp thử nghiệm. 
2. Quét mảng một lần trong khi theo dõi giá trị tối thiểu gặp phải. Ý tưởng là để duy trì ứng cử viên tốt nhất có thể cho giá trị trung bình của tập hợp con cuối cùng. 
3. Sau khi xử lý tất cả các phần tử, xuất ra giá trị tối thiểu. Điều này tương ứng với việc chọn một tập hợp con chỉ bao gồm phần tử nhỏ nhất đó. 

### Tại sao nó hoạt động 

Bất kỳ tập hợp con nào cũng có giá trị trung bình ít nhất là phần tử nhỏ nhất của nó, vì giá trị trung bình của các số không thể nhỏ hơn giá trị nhỏ nhất trong tập hợp đó. Do đó, mức trung bình của mọi tập hợp con hợp lệ sẽ bị giới hạn thấp hơn bởi mức tối thiểu toàn cục của mảng. Giới hạn này có thể đạt được bằng cách chọn tập hợp con chỉ chứa phần tử tối thiểu đó, do đó không có cấu trúc nào khác có thể tạo ra giá trị nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    print(min(arr))
```Giải pháp hoàn toàn dựa vào việc tính toán mức tối thiểu duy nhất cho mỗi trường hợp thử nghiệm. Các trường hợp thử nghiệm lặp lại đảm bảo chúng tôi tôn trọng sự phân tách đầu vào. sử dụng`min(arr)`là an toàn vì kích thước mảng cho mỗi trường hợp thử nghiệm ít nhất là một, vì vậy chúng tôi không bao giờ vi phạm yêu cầu rằng tập hợp con phải không trống. 

Chi tiết triển khai chính là chúng tôi không cố gắng xây dựng hoặc theo dõi các tập hợp con. Bất kỳ nỗ lực nào như vậy đều là chi phí không cần thiết do việc giảm toán học xuống một giá trị duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
800 4000 969
```Chúng tôi theo dõi mức tối thiểu trong khi quét. 

| Bước | Giá trị | Tối thiểu hiện tại | 
| --- | --- | --- | 
| 1 | 800 | 800 | 
| 2 | 4000 | 800 | 
| 3 | 969 | 800 | 

Kết quả là`800`. Điều này cho thấy rằng mặc dù tồn tại các giá trị lớn hơn nhưng chúng không thể giảm mức trung bình có thể đạt được xuống dưới phần tử nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
4
5 5 5 5
```| Bước | Giá trị | Tối thiểu hiện tại | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 5 | 5 | 
| 3 | 5 | 5 | 
| 4 | 5 | 5 | 

Đầu ra là`5`. Điều này thể hiện trường hợp có giá trị bằng nhau trong đó mọi tập hợp con đều có mức trung bình giống hệt nhau, xác nhận rằng việc xóa không ảnh hưởng đến kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | Chúng tôi quét từng mảng một lần để tính giá trị tối thiểu | 
| Không gian | O(1) thêm | Chỉ có mức tối thiểu đang chạy được lưu trữ | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm tối đa là 100000, do đó, một lần tuyến tính duy nhất cho mỗi trường hợp thử nghiệm sẽ phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        out.append(str(min(arr)))
    return "\n".join(out)

# provided sample-style tests
assert run("1\n3\n800 4000 969\n") == "800"
assert run("1\n4\n5 5 5 5\n") == "5"

# custom cases
assert run("1\n1\n1234\n") == "1234", "single element"
assert run("1\n5\n10 9 8 7 6\n") == "6", "strictly decreasing"
assert run("1\n5\n6 7 8 9 10\n") == "6", "strictly increasing"
assert run("2\n3\n2 100 3\n4\n1 1000 1000 1000\n") == "2\n1", "multiple tests"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1234 | trường hợp ranh giới tối thiểu | 
| mảng giảm dần | 6 | phút ở cuối | 
| mảng tăng dần | 6 | phút khi bắt đầu | 
| nhiều bài kiểm tra | 2/1 | tính đúng đắn trong các trường hợp | 

## Vỏ cạnh 

Mảng một phần tử là ràng buộc chặt chẽ nhất: tập hợp con hợp lệ duy nhất là chính phần tử đó, do đó thuật toán sẽ xuất trực tiếp phần tử đó. Đối với đầu vào`[1234]`, quá trình quét đặt giá trị tối thiểu là 1234 và trả về giá trị đó mà không có bất kỳ sự mơ hồ nào. 

Đối với các mảng có giá trị tối thiểu xuất hiện nhiều lần, chẳng hạn như`[2, 100, 2, 50]`, thuật toán gặp 2 ở nhiều vị trí nhưng mức tối thiểu chạy vẫn là 2 xuyên suốt và đầu ra là 2. Bất kỳ tập hợp con nào chứa các phần tử khác không thể giảm mức trung bình xuống dưới 2, do đó các bản sao không thay đổi hành vi. 

Đối với các mảng tăng nghiêm ngặt như`[1, 2, 3, 4]`, mức tối thiểu được tìm thấy ngay tại phần tử đầu tiên. Mặc dù các phần tử sau này lớn hơn nhưng thuật toán không bao giờ thay thế giá trị tốt nhất hiện tại mà bỏ qua tất cả các phần bổ sung một cách chính xác. 

Những trường hợp này xác nhận rằng nghiệm chỉ phụ thuộc vào việc xác định mức tối thiểu toàn cục và không có đặc tính cấu trúc nào của việc sắp xếp hoặc nhóm ảnh hưởng đến kết quả cuối cùng.
