---
title: "CF 103765A - \u793e\u56e2\u62db\u65b0"
description: "Chúng ta có một nhóm học sinh, mỗi học sinh có thể được phân biệt nhưng có vai trò giống hệt nhau. Chúng tôi được phép thành lập nhiều nhóm, trong đó mỗi nhóm chỉ là một tập hợp con được chọn của những học sinh này. Mỗi nhóm phải thỏa mãn hai ràng buộc về cấu trúc."
date: "2026-07-02T08:54:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103765
codeforces_index: "A"
codeforces_contest_name: "2022 Collegiate Programming Contest of Xiangtan University"
rating: 0
weight: 103765
solve_time_s: 56
verified: true
draft: false
---

[CF 103765A - \u793e\u56e2\u62db\u65b0](https://codeforces.com/problemset/problem/103765/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm học sinh, mỗi học sinh có thể được phân biệt nhưng có vai trò giống hệt nhau. Chúng tôi được phép thành lập nhiều nhóm, trong đó mỗi nhóm chỉ là một tập hợp con được chọn của những học sinh này. 

Mỗi nhóm phải thỏa mãn hai ràng buộc về cấu trúc. Đầu tiên, số học sinh trong một nhóm bất kỳ phải là số lẻ. Thứ hai, nếu chúng ta chọn hai nhóm khác nhau thì số học sinh của họ phải chẵn. 

Nhiệm vụ là xác định số lượng tối đa các nhóm như vậy có thể được thành lập từ một nhóm n sinh viên. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm độc lập, mỗi trường hợp cho một giá trị n. Đối với mỗi n chúng ta phải xuất ra số lượng nhóm hợp lệ tối đa có thể. 

Các ràng buộc cho phép tối đa 200000 trường hợp thử nghiệm và n lên tới 1.000.000, điều này ngay lập tức loại trừ bất kỳ cấu trúc nào cố gắng liệt kê rõ ràng các tập hợp con hoặc kiểm tra các giao điểm theo cặp. Ngay cả việc lưu trữ tất cả các nhóm một cách rõ ràng cũng sẽ quá lớn nếu chúng ta thử bất kỳ thứ gì siêu tuyến tính cho mỗi trường hợp thử nghiệm. Giải pháp phải giảm từng trường hợp thử nghiệm xuống một lượng lý luận không đổi. 

Một cách giải thích ngây thơ sẽ đề xuất việc cố gắng xây dựng các tập hợp con một cách tham lam và xác minh các ràng buộc khi chúng ta tiếp tục. Ví dụ: người ta có thể cố gắng thêm các tập hợp con trong khi vẫn duy trì các điều kiện chẵn lẻ trên các giao điểm với tất cả các tập hợp con đã chọn trước đó. Điều này nhanh chóng trở nên tốn kém vì mỗi tập hợp con mới yêu cầu quét tất cả các tập hợp con hiện có và tính toán các giao điểm, dẫn đến hành vi bậc hai về số lượng nhóm. 

Một vấn đề tinh tế hơn sẽ xuất hiện nếu chúng ta thử xây dựng tập hợp con ngẫu nhiên hoặc tham lam: các ràng buộc chẵn lẻ có tính toàn cục. Một lựa chọn hợp lệ cục bộ của một tập hợp con có thể dễ dàng phá vỡ quy tắc giao nhau chẵn với các tập hợp con trước đó và việc phát hiện điều này đòi hỏi phải so sánh đầy đủ nhiều lần. 

Một trường hợp thất bại cụ thể đối với cách tiếp cận đơn giản là khi n nhỏ nhưng chúng ta cố gắng liệt kê các tập hợp con có kích thước 1, 3, 5, v.v. Ví dụ: với n = 3, nếu chúng ta chọn các tập con {1}, {2} và {1,2,3} thì các giao điểm không đồng đều và chiến lược tham lam có thể chấp nhận các tập hợp không tương thích trừ khi mọi cặp đều được kiểm tra. 

Khó khăn thực sự là điều kiện trên các giao điểm không phải là tổ hợp theo nghĩa thông thường mà là đại số, và việc bỏ qua cấu trúc đó dẫn đến mô phỏng phức tạp không cần thiết. 

## Phương pháp tiếp cận 

Bước quan trọng là diễn giải lại vấn đề theo tính chẵn lẻ. 

Biểu diễn mỗi nhóm dưới dạng một vectơ nhị phân có độ dài n, trong đó tọa độ thứ i là 1 nếu học sinh i thuộc nhóm và 0 nếu ngược lại. Theo cách biểu diễn này, kích thước của một nhóm là tổng các phần tử của nó và kích thước giao điểm giữa hai nhóm trở thành tích vô hướng của các vectơ của chúng. 

Điều kiện mọi nhóm có kích thước lẻ có nghĩa là tổng tọa độ của mỗi vectơ là 1 modulo 2. Điều kiện giao điểm của hai nhóm bất kỳ có kích thước chẵn có nghĩa là tích vô hướng của hai vectơ phân biệt bất kỳ là 0 modulo 2. 

Vì vậy, chúng tôi đang tìm kiếm càng nhiều vectơ càng tốt trong không gian vectơ GF(2)^n sao cho mọi vectơ đều có giá trị chẵn lẻ 1 và mọi cặp đều trực giao theo tích bên trong tiêu chuẩn. 

Tại thời điểm này cấu trúc trở thành đại số tuyến tính trên GF(2). Ràng buộc rằng các vectơ trực giao theo cặp ngay lập tức hàm ý tính độc lập tuyến tính: nếu tổ hợp tuyến tính của các vectơ như vậy bằng 0, việc lấy tích vô hướng với một trong số chúng sẽ gây ra mâu thuẫn vì tích tự chấm của nó là 1 trong khi tất cả các số hạng chéo đều biến mất. 

Do đó, bất kỳ họ k nhóm hợp lệ nào cũng tương ứng với k vectơ độc lập tuyến tính trong không gian vectơ n chiều. Điều này ngay lập tức cho k ≤ n.

Giới hạn trên cũng có thể đạt được. Các vectơ cơ sở tiêu chuẩn e1, e2, ..., en mỗi vectơ đại diện cho một nhóm bao gồm một học sinh riêng biệt. Mỗi nhóm như vậy có kích thước 1, là số lẻ và các giao điểm giữa các nhóm riêng biệt là trống, là số chẵn. Vì vậy, tất cả các ràng buộc đều được thỏa mãn và chúng tôi đạt được n nhóm. 

Không có công trình nào có thể vượt quá giới hạn này và cơ sở tiêu chuẩn cho thấy giới hạn này rất chặt chẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Xây dựng tập hợp con Brute Force | hàm mũ/ít nhất là O(2^n) | O(2^n) | Quá chậm | 
| Định dạng lại đại số tuyến tính | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case T. Mỗi test case độc lập và chỉ phụ thuộc vào n. 
2. Với mỗi n, thừa nhận rằng số lượng nhóm hợp lệ tối đa bị chặn ở trên bởi n do tính độc lập tuyến tính đối với GF(2). Điều này tránh được nhu cầu xây dựng các nhóm một cách rõ ràng. 
3. Xuất trực tiếp n làm đáp án cho test case. 

Lý do duy nhất cần có cho mỗi trường hợp thử nghiệm là giới hạn cấu trúc này. Không cần mô phỏng hoặc xây dựng. 

### Tại sao nó hoạt động 

Mỗi nhóm có thể được xem như một vectơ trong không gian vectơ n chiều trên GF(2). Điều kiện giao điểm là chẵn buộc tích số chấm theo cặp bằng 0, hàm ý tính trực giao. Trong cài đặt này, bất kỳ tập hợp vectơ khác 0 trực giao từng cặp nào đều độc lập tuyến tính. Vì không gian có chiều n nên không thể tồn tại nhiều hơn n vectơ như vậy. Cơ sở tiêu chuẩn cung cấp n vectơ hợp lệ thỏa mãn cả điều kiện kích thước lẻ và điều kiện trực giao, do đó giới hạn là chặt chẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(n)
```Giải pháp hoàn toàn dựa vào quan sát rằng mỗi trường hợp thử nghiệm giảm xuống một đầu ra số nguyên duy nhất. Không có tính toán biên hoặc trạng thái trung gian, giúp duy trì việc triển khai ở mức tối thiểu và tránh mọi rủi ro về vấn đề tràn hoặc lập chỉ mục. 

## Ví dụ đã hoạt động 

Vì tuyên bố ban đầu không cung cấp đầu vào mẫu rõ ràng nên chúng tôi xem xét các trường hợp minh họa. 

### Ví dụ 1 

đầu vào: 

n = 1 

Chúng ta chỉ có một học sinh, vì vậy nhóm không trống duy nhất có thể là {1}. Nhóm này có kích thước 1, đó là số lẻ. Không có cặp nhóm nào vi phạm ràng buộc giao lộ nên tối đa là 1. 

Đầu ra: 

| Bước | n | Số nhóm hợp lệ | 
| --- | --- | --- | 
| ban đầu | 1 | 0 | 
| thêm {1} | 1 | 1 | 

Điều này khẳng định việc xây dựng đạt được giới hạn. 

Đầu ra: 1 

### Ví dụ 2 

đầu vào: 

n = 4 

Chúng ta có thể xây dựng bốn nhóm: {1}, {2}, {3}, {4}. Mỗi cái có kích thước lẻ và hai cái bất kỳ là rời nhau, vì vậy giao điểm là 0 là số chẵn. 

| Bước | nhóm được thành lập | hợp lệ | 
| --- | --- | --- | 
| bắt đầu | ∅ | hợp lệ | 
| thêm {1} | {1} | hợp lệ | 
| thêm {2} | {1},{2} | hợp lệ | 
| thêm {3} | {1},{2},{3} | hợp lệ | 
| thêm {4} | {1},{2},{3},{4} | hợp lệ | 

Chúng tôi đạt được 4 nhóm, phù hợp với n. 

Đầu ra: 4 

Những ví dụ này cho thấy rằng công trình có tỷ lệ trực tiếp với n và không bị suy giảm do các hạn chế về giao lộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Một đầu ra thời gian không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không cần cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả đối với 200000 trường hợp thử nghiệm, chúng tôi chỉ thực hiện đọc và ghi một số nguyên duy nhất cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(n))
    return "\n".join(out)

# provided-like samples
assert run("1\n3\n") == "3"
assert run("2\n1\n4\n") == "1\n4"

# minimum size
assert run("1\n3\n") == "3"

# multiple cases
assert run("3\n3\n4\n5\n") == "3\n4\n5"

# large value
assert run("1\n1000000\n") == "1000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ n | n | độ đúng cơ sở | 
| nhiều giá trị n | mỗi n không thay đổi | tính độc lập trong mỗi bài kiểm tra | 
| lớn | n | không tràn/mở rộng quy mô | 
| hợp lệ tối thiểu n=3 | 3 | độ đúng ranh giới | 

## Vỏ cạnh 

Một mối quan tâm tiềm ẩn là liệu các ràng buộc có thể tạo ra sự tương tác phức tạp hơn giữa các nhóm khi n nhỏ hay không. Ví dụ, khi n = 3, người ta có thể nghi ngờ rằng các tập con có kích thước lẻ là có hạn. Tuy nhiên, việc xây dựng sử dụng các tập hợp con một phần tử vẫn hoạt động trơn tru. 

Với n = 3, chọn {1}, {2}, {3} sẽ có ba nhóm. Mỗi nhóm có cỡ 1, thỏa mãn độ lẻ. Bất kỳ cặp nào cũng có giao điểm trống, tức là chẵn. Thuật toán đưa ra 3, phù hợp với cấu trúc rõ ràng. 

Đối với n lớn hơn, chẳng hạn như n = 1.000.000, lý do tương tự được áp dụng mà không có bất kỳ thay đổi cấu trúc nào. Giải pháp không phụ thuộc vào phép liệt kê tổ hợp nên không có nguy cơ xảy ra trường hợp góc ẩn phát sinh từ tương tác tập hợp con.
