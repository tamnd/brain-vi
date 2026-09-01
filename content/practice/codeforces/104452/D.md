---
title: "CF 104452D - Giáo sư R. trung vị"
description: "Chúng ta được cung cấp một danh sách các số nguyên và chúng ta cần chọn một phần tử theo quy tắc phụ thuộc vào giá trị nhỏ nhất và lớn nhất trong danh sách."
date: "2026-06-30T14:42:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104452
codeforces_index: "D"
codeforces_contest_name: "ICPC Central Russia Regional Contest - 2020"
rating: 0
weight: 104452
solve_time_s: 91
verified: true
draft: false
---

[CF 104452D - Giáo sư R. Trung bình](https://codeforces.com/problemset/problem/104452/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên và chúng ta cần chọn một phần tử theo quy tắc phụ thuộc vào giá trị nhỏ nhất và lớn nhất trong danh sách. 

Thay vì sắp xếp toàn bộ mảng và lấy trung vị cổ điển, trước tiên chúng ta tính giá trị tham chiếu, là giá trị trung bình số học của phần tử tối thiểu và tối đa. Sau đó, chúng tôi tìm phần tử trong mảng gần nhất với giá trị tham chiếu này. Khoảng cách được đo theo nghĩa chênh lệch tuyệt đối thông thường. Nếu nhiều phần tử gần nhau thì ta chọn giá trị nhỏ nhất trong số đó. 

Vì vậy, nhiệm vụ không phải là sắp xếp thứ tự vị trí trong một mảng được sắp xếp mà là về độ gần với điểm giữa của các giá trị cực trị. 

Kích thước đầu vào lên tới 100.000 phần tử với các giá trị có độ lớn lên tới khoảng 2·10^9. Điều này ngay lập tức loại trừ các giải pháp tính toán lại bất cứ thứ gì đắt tiền cho mỗi ứng viên hoặc thử tất cả các cặp. Bất cứ điều gì bậc hai, chẳng hạn như so sánh mọi phần tử với mọi phần tử khác hoặc sắp xếp bên trong các vòng lặp lồng nhau, sẽ quá chậm. Quét tuyến tính sau quá trình tiền xử lý đơn giản là hướng khả thi duy nhất. 

Một số trường hợp đặc biệt quan trọng ở đây. 

Nếu tất cả các phần tử đều bằng nhau thì giá trị tối thiểu và tối đa trùng nhau nên giá trị tham chiếu chính xác là con số đó. Mọi phần tử đều gần nhau như nhau nên câu trả lời phải có cùng giá trị. Bất kỳ giải pháp nào xử lý sai tính toán điểm giữa hoặc các ràng buộc dấu phẩy động vẫn có thể phá vỡ trường hợp này. 

Nếu mảng chứa các giá trị được đặt đối xứng xung quanh điểm giữa, ví dụ [1, 3] hoặc [1, 2, 3], nhiều phần tử có thể liên kết với nhau về khoảng cách. Quy tắc ràng buộc buộc chúng ta phải chọn ứng cử viên nhỏ nhất trong số đó, vì vậy chúng ta phải theo dõi rõ ràng các mối quan hệ, không chỉ giữ bất kỳ giá trị gần nhất nào. 

Cuối cùng, vì các giá trị có thể lớn nên việc tính toán mid = (min + max)/2 dưới dạng số dấu phẩy động có thể gây ra các vấn đề về độ chính xác nếu thực hiện bất cẩn. Tuy nhiên, chúng ta hoàn toàn không cần dấu phẩy động nếu so sánh khoảng cách bằng đại số. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp gợi ý tính toán mức tối thiểu và tối đa của mảng, sau đó kiểm tra mọi phần tử để xem nó gần điểm giữa của chúng đến mức nào. Ý tưởng brute-force rất đơn giản: tính toán tối thiểu và tối đa, tính điểm giữa, sau đó lặp lại tất cả các phần tử và theo dõi phần tử có chênh lệch tuyệt đối tối thiểu so với điểm giữa đó. 

Điều này đã đưa ra giải pháp O(n), do đó không cần phải sắp xếp hoặc so sánh lồng nhau. Điều tinh tế duy nhất là cách chúng tôi so sánh khoảng cách mà không gây ra lỗi dấu phẩy động và cách chúng tôi xử lý các mối quan hệ một cách nhất quán. 

Quan sát quan trọng là điểm giữa được cố định khi biết cực tiểu và cực đại, do đó, bài toán giảm xuống còn một bài toán chọn một lượt: chọn cực tiểu hóa phần tử |a[i] - (min + max)/2|. Vì các phép so sánh với một ngưỡng không đổi có thể được viết lại mà không cần chia, nên chúng ta có thể tránh hoàn toàn số float bằng cách so sánh 2 * a[i] với (min + max). Điều này giữ mọi thứ ở dạng số nguyên và duy trì thứ tự chính xác. 

Mô hình tinh thần brute-force hoạt động vì chúng ta chỉ cần cực trị toàn cục và một lần truyền qua mảng. Nó chỉ thất bại nếu được thực hiện bằng phép tính điểm giữa dấu phẩy động đơn giản hoặc nếu việc phá vỡ ràng buộc không được xử lý rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (tối thiểu/tối đa + quét) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (tương tự với so sánh an toàn số nguyên) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các số và tính cả giá trị nhỏ nhất và giá trị lớn nhất trong mảng. Điều này thiết lập các điểm cuối của khoảng tham chiếu xác định điểm giữa đích. 
2. Tính tổng các điểm cuối này, đại diện cho hai lần điểm giữa. Chúng ta tránh phép chia vì chúng ta chỉ cần so sánh chứ không cần giá trị trung điểm thực tế. 
3. Khởi tạo một biến để lưu trữ ứng viên tốt nhất và một biến khác để lưu trữ khoảng cách tốt nhất được nhìn thấy cho đến nay. Khoảng cách tốt nhất bắt đầu từ vô cực. 
4. Lặp lại từng phần tử trong mảng. Đối với mỗi phần tử, tính khoảng cách tuyệt đối của nó đến điểm giữa ở dạng số nguyên tỷ lệ, so sánh |2 * a[i] - (min + max)|. Điều này đo thứ tự tương tự như khoảng cách đến điểm giữa thực sự. 
5. Nếu khoảng cách này nhỏ hơn khoảng cách tốt nhất cho đến nay, hãy cập nhật cả khoảng cách tốt nhất và giá trị ứng cử viên tốt nhất. 
6. Nếu khoảng cách bằng khoảng cách tốt nhất, chỉ cập nhật ứng viên nếu giá trị hiện tại nhỏ hơn. Điều này thực thi quy tắc ràng buộc. 
7. Sau khi xử lý tất cả các phần tử, xuất ra ứng viên được lưu trữ. 

### Tại sao nó hoạt động 

Điểm giữa xuất phát từ mức tối thiểu và tối đa toàn cầu là hằng số cố định độc lập với cấu trúc mảng. Mọi phần tử được đánh giá hoàn toàn bằng khoảng cách của nó với hằng số này, do đó, vấn đề giảm xuống còn việc chọn một argmin toàn cục trên một hàm xác định. Việc chuyển đổi sang giá trị nhân đôi vẫn giữ nguyên thứ tự vì phép nhân với 2 là đơn điệu và không ảnh hưởng đến phép so sánh. Quy tắc ràng buộc được thực thi rõ ràng trong quá trình quét, đảm bảo lựa chọn xác định giữa các ứng cử viên có khoảng cách bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mn = min(a)
    mx = max(a)
    target_sum = mn + mx

    best_val = None
    best_dist = None

    for x in a:
        dist = abs(2 * x - target_sum)
        if best_dist is None or dist < best_dist or (dist == best_dist and x < best_val):
            best_dist = dist
            best_val = x

    print(best_val)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách trích xuất cực trị tổng thể, xác định rõ điểm giữa tham chiếu. Thay vì tính toán trực tiếp điểm giữa, nó hoạt động với biểu thức nhân đôi để giữ cho tất cả số học được tích phân và chính xác. Vòng lặp duy trì một ứng cử viên tốt nhất đang chạy, chỉ cập nhật nó khi tìm thấy một khoảng cách tốt hơn hoặc khi một điểm hòa bị phá vỡ bởi một giá trị nhỏ hơn. 

Một lỗi triển khai phổ biến là tính toán`(mn + mx) / 2`sử dụng số học dấu phẩy động và sau đó so sánh khoảng cách bằng cách sử dụng số float. Điều đó có thể gây ra lỗi làm tròn khi giá trị lớn. Một lỗi nhỏ khác là không thực thi nghiêm ngặt quy tắc ràng buộc, dẫn đến kết quả đầu ra không chính xác khi nhiều giá trị cách đều nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 1 1 1
```Ở đây mn = 1 và mx = 1, do đó điểm giữa tham chiếu là 1. 

| Bước | x | 2x | mn+mx | quận | tốt nhất_val | best_dist | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | 2 | - | - | thông tin | 
| 1 | 1 | 2 | 2 | 0 | 1 | 0 | 
| 2 | 1 | 2 | 2 | 0 | 1 | 0 | 
| ... | 1 | 2 | 2 | 0 | 1 | 0 | 

Mọi phần tử đều giống hệt nhau nên mọi khoảng cách đều bằng không. Quy tắc tie-break luôn giữ giá trị nhỏ nhất, vẫn là 1. Điều này xác nhận tính đúng đắn trong trường hợp suy biến trong đó min bằng max. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Ở đây mn = 1, mx = 3 nên trung điểm là 2. 

| Bước | x | 2x | mn+mx | quận | tốt nhất_val | best_dist | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | 4 | - | - | thông tin | 
| 1 | 1 | 2 | 4 | 2 | 1 | 2 | 
| 2 | 2 | 4 | 4 | 0 | 2 | 0 | 
| 3 | 3 | 6 | 4 | 2 | 2 | 0 | 

Yếu tố 2 chính xác là điểm giữa nên trở thành ứng cử viên sáng giá nhất ngay lập tức. Điều này chứng tỏ rằng các trận đấu chính xác chiếm ưu thế hơn tất cả những trận đấu khác bất kể hòa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính toán tối thiểu/tối đa và một lượt để chọn phần tử tốt nhất | 
| Không gian | O(1) | chỉ một số biến được duy trì bất kể kích thước đầu vào | 

Các ràng buộc cho phép lên tới 100.000 phần tử, do đó việc quét tuyến tính nằm trong giới hạn một cách thoải mái. Giải pháp chỉ thực hiện công việc liên tục trên mỗi phần tử và tránh việc sắp xếp hoặc lặp lại các bước vượt quá thời gian tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    mn = min(a)
    mx = max(a)
    target_sum = mn + mx

    best_val = None
    best_dist = None

    for x in a:
        dist = abs(2 * x - target_sum)
        if best_dist is None or dist < best_dist or (dist == best_dist and x < best_val):
            best_dist = dist
            best_val = x

    return str(best_val)

# provided samples
assert run("5\n1 1 1 1 1\n") == "1"
assert run("3\n1 2 3\n") == "2"

# custom cases
assert run("1\n10\n") == "10", "single element"
assert run("2\n1 100\n") == "1", "tie both equal distance choose smaller"
assert run("4\n5 1 9 3\n") == "5", "midpoint selection"
assert run("6\n2 2 2 2 2 2\n") == "2", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | xử lý kích thước tối thiểu | 
| 1 100 | 1 | sự đúng đắn của sự ràng buộc | 
| 5 1 9 3 | 5 | hành vi trung điểm chung | 
| tất cả đều bình đẳng | 2 | mảng thống nhất suy biến | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, min bằng max và điểm giữa mục tiêu thu gọn về cùng giá trị đó. Bộ thuật toán`target_sum = 2 * x`, vì vậy mọi phần tử đều tạo ra khoảng cách bằng không. Bởi vì quy tắc tie-break ưu tiên các giá trị nhỏ hơn và tất cả các giá trị đều bằng nhau nên phần tử gặp đầu tiên vẫn hợp lệ và câu trả lời cuối cùng là giá trị lặp lại đó. 

Khi có hai phần tử cách đều nhau tính từ điểm giữa, chẳng hạn như`[1, 3]`, cả hai đều mang lại khoảng cách 2 từ điểm giữa 2. Thuật toán so sánh 1 đầu tiên, sau đó là 3. Sau khi xử lý 1, best_val trở thành 1. Khi xử lý 3, khoảng cách liên kết nhưng 3 không nhỏ hơn 1 nên bị bỏ qua. Đầu ra vẫn là 1, phù hợp với quy tắc bắt buộc.
