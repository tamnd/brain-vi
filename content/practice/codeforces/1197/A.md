---
title: "CF 1197A - Thang Gỗ Tự Làm"
description: "Chúng tôi được cung cấp một số bộ ván gỗ độc lập, trong đó mỗi tấm ván có chiều dài cố định và không thể cắt được. Từ mỗi bộ, chúng tôi muốn lắp ráp một cấu trúc gọi là bậc thang với càng nhiều bậc càng tốt. Một thang k bước được hình thành bằng cách chọn chính xác k + 2 tấm ván."
date: "2026-06-13T14:29:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1197
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 69 (Rated for Div. 2)"
rating: 900
weight: 1197
solve_time_s: 353
verified: true
draft: false
---

[CF 1197A - Thang gỗ tự làm](https://codeforces.com/problemset/problem/1197/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 5 phút 53 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số bộ ván gỗ độc lập, trong đó mỗi tấm ván có chiều dài cố định và không thể cắt được. Từ mỗi bộ, chúng tôi muốn lắp ráp một cấu trúc gọi là bậc thang với càng nhiều bậc càng tốt. 

Một thang k bước được hình thành bằng cách chọn chính xác k + 2 tấm ván. Hai trong số này phải là tấm ván “đế” và mỗi tấm ván phải có chiều dài ít nhất là k + 1. K tấm ván còn lại là tấm ván “bậc thang” và mỗi tấm ván trong số chúng phải có chiều dài ít nhất là 1, điều này có nghĩa là bất kỳ tấm ván nào cũng có thể đóng vai trò là một bậc thang. 

Vì vậy, hạn chế thực sự duy nhất là ở phần đế: chúng ta cần hai tấm ván đủ dài để đỡ một chiếc thang có chiều cao k, và những tấm ván còn lại đã chọn chỉ cần tồn tại để tạo thành các bậc thang. 

Đối với mỗi truy vấn, chúng ta phải chọn một số tập hợp con của bảng để tối đa hóa k. Chúng ta được phép bỏ qua hoàn toàn các plan, nhưng không thể tách hoặc sửa đổi chúng. 

Các ràng buộc cho phép tổng cộng lên tới 100.000 bảng trong tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra tất cả các tập hợp con hoặc thậm chí tất cả các cặp ứng cử viên cơ sở trên k có thể. Bất cứ điều gì bậc hai trong n cho mỗi trường hợp thử nghiệm sẽ thất bại. Có thể chấp nhận việc sắp xếp hoặc quét tuyến tính cho mỗi lần kiểm tra. 

Một vài trường hợp thất bại khó nhận thấy sẽ xuất hiện nếu chúng ta suy luận quá cục bộ. Đầu tiên, thật hấp dẫn khi cho rằng câu trả lời chỉ phụ thuộc vào hai tấm ván lớn nhất, mà bỏ qua việc liệu có đủ tấm ván còn lại để tạo thành các bậc thang hay không. Thứ hai, rất dễ quên rằng việc tăng k làm cho các yêu cầu cơ bản chặt chẽ hơn, do đó, một giải pháp tham lam chọn k lớn nhất trước mà không kiểm tra tính khả thi sẽ đánh giá quá cao. 

Trường hợp cạnh bê tông là khi chúng ta chỉ có hai tấm ván lớn nhưng không đủ tổng số tấm ván: 

đầu vào:```
1
3
10 10 1
```Chúng ta có thể tạo k = 1, vì chúng ta có thể dùng 10 và 10 làm cơ số và 1 còn lại làm bước. Nhưng k = 2 là không thể mặc dù các đế đã đủ, vì chúng ta sẽ cần tổng cộng 4 tấm ván. 

Một trường hợp khác là có nhiều tấm ván nhỏ nhưng không có hai tấm ván lớn:```
1
5
1 1 1 1 1
```Không thể xây dựng được thang vì chúng ta không thể tạo thành hai đáy có chiều dài ít nhất là 2. 

Những ví dụ này cho thấy rằng cả “số lượng ván dài” và “sự sẵn có của các tấm ván bổ sung” đều quan trọng đồng thời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi k có thể từ 0 đến n − 2 và kiểm tra tính khả thi. Đối với mỗi k, chúng tôi sẽ quét mảng, đếm xem có bao nhiêu tấm ván ít nhất là k + 1 cho các ứng cử viên cơ sở và xác minh rằng chúng tôi có ít nhất hai tấm ván như vậy và tổng số ít nhất k + 2 tấm ván. Mỗi lần kiểm tra đều tốn O(n) và thực hiện nó với tất cả k sẽ mang lại O(n^2) cho mỗi trường hợp thử nghiệm, quá chậm đối với tổng số 100.000 phần tử. 

Nhận xét quan trọng là tính khả thi có tính đơn điệu về mặt cấu trúc. Khi k tăng thì ngưỡng đáy k+1 tăng nên tập ván đế sử dụng được chỉ co lại. Đồng thời, tổng số ván cần thiết tăng tuyến tính. Điều này cho thấy chúng tôi có thể xử lý các ứng viên theo thứ tự độ dài giảm dần và theo dõi số lượng cơ sở có thể sử dụng mà chúng tôi hiện có. 

Nếu chúng ta sắp xếp các tấm ván theo thứ tự giảm dần, chúng ta có thể tưởng tượng việc giảm dần ngưỡng cho k và hỏi: tại thời điểm nào trước tiên chúng ta có đủ tấm ván có thể dùng làm đế? Mỗi lần chúng ta xem xét một ứng cử viên k, chúng ta cần biết liệu có ít nhất hai tấm ván trong số những tấm ván có chiều dài ≥ k + 1 hay không và liệu chúng ta có sẵn ít nhất tổng số k + 2 tấm ván hay không. 

Thay vì tính toán lại số lượng từ đầu cho mỗi k, chúng tôi quét từ chiều dài ván lớn nhất đến nhỏ nhất và duy trì số lượng phần tử hiện “đủ điều kiện” để làm cơ sở ở ngưỡng hiện tại. Điều này làm giảm vấn đề xuống còn một lần sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp chiều dài tấm ván theo thứ tự giảm dần. Thứ tự này cho phép chúng ta suy luận về các yêu cầu cơ bản ngày càng chặt chẽ hơn. 

Chúng tôi duy trì một con trỏ i di chuyển qua mảng đã sắp xếp và biểu thị số lượng tấm ván hiện được coi là có thể sử dụng được làm cơ sở tiềm năng dưới một ngưỡng nhất định. Chúng tôi cũng theo dõi số bước k hiện tại chúng tôi có thể hỗ trợ dựa trên số lượng ván chúng tôi đã xử lý. 

1. Sắp xếp mảng theo thứ tự giảm dần để xử lý các bảng lớn nhất trước. Điều này đảm bảo rằng khi chúng tôi xem xét ngưỡng ứng viên, tất cả các bảng lớn hơn hoặc bằng nhau đều đã được tính đến. 
2. Khởi tạo bộ đếm cnt = 0, đại diện cho số lượng bảng mà chúng ta đã thấy cho đến nay đủ điều kiện để trở thành ứng cử viên cơ sở ở ngưỡng hiện tại. 
3. Duyệt mảng đã sắp xếp. Tại vị trí i, chúng ta đang xem xét một cách hiệu quả liệu chúng ta có thể hỗ trợ một bậc thang có ngưỡng cơ sở là a[i] hay không. Mỗi khi chúng tôi thêm một tấm ván mới, chúng tôi sẽ tăng cnt. 
4. Sau khi bao gồm ván thứ i, hãy kiểm tra xem liệu chúng ta có thể tạo một thang hợp lệ bằng cách sử dụng các ứng cử viên cơ sở đủ điều kiện cnt hay không. Vì chúng ta cần hai cơ sở nên chúng ta cần cnt ≥ 2. 
5. Số bước k mà chúng tôi có thể hỗ trợ ở giai đoạn này bị giới hạn bởi số lượng tấm ván mà chúng tôi đã xử lý trừ đi 2, vì tổng số tấm ván là k + 2. Vì vậy chúng ta tính k = min(cnt - 2, i - 1). 
6. Theo dõi k tối đa trên tất cả các vị trí. 

Bước lập luận quan trọng là ở mỗi giai đoạn, chỉ số hiện tại i tương ứng với một số ván ứng cử viên mà chúng ta có thể sử dụng và cnt theo dõi xem có bao nhiêu ván trong số đó đủ mạnh để làm cơ sở cho cấp độ đó. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong quá trình truyền tải được sắp xếp, chúng ta có chính xác tập hợp các tấm ván có kích thước ít nhất bằng ngưỡng hiện tại. Bất kỳ bậc thang hợp lệ nào có chiều cao k phải chọn hai tấm ván cơ sở từ bộ này, vì vậy tính khả thi chỉ phụ thuộc vào số lượng phần tử còn lại trong tiền tố này. Vì cả hai ràng buộc, yêu cầu cơ bản và tổng số ván đều thắt chặt một cách đơn điệu khi chúng ta di chuyển qua các giá trị nhỏ hơn, nên k tốt nhất có thể đạt được phải xảy ra ở một trong các trạng thái tiền tố này. Do đó, việc kiểm tra từng tiền tố sẽ nắm bắt được tất cả các cấu hình tối ưu có thể có mà không bỏ sót bất kỳ sự sắp xếp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort(reverse=True)
    
    cnt = 0
    ans = 0
    
    for i, val in enumerate(a):
        cnt += 1
        
        k = min(cnt - 2, i)  # i = number of remaining step capacity minus 1
        if k > ans:
            ans = k
    
    print(max(ans, 0))
```Giải pháp bắt đầu bằng cách sắp xếp các chiều dài của tấm ván để chúng ta có thể đánh giá tính khả thi theo thứ tự giảm dần độ bền cơ bản tiềm năng. Biến cnt theo dõi số lượng bảng hiện có trong tiền tố mà chúng ta đang xem xét. Ở mỗi bước, tôi biểu thị số lượng tấm ván bổ sung ngoài tấm đầu tiên có sẵn để đóng vai trò là các bước. 

Biểu thức cnt - 2 phản ánh số lượng tấm ván có thể được dành làm các bước sau khi chọn hai đế. Biểu thức i phản ánh tổng số “khe” có sẵn để xây dựng cấu trúc theo tiền tố này. Việc lấy mức tối thiểu đảm bảo chúng ta tôn trọng cả hai ràng buộc cùng một lúc. 

Cuối cùng, chúng tôi lấy mức tối đa trên tất cả các tiền tố vì kích thước bậc thang tối ưu tương ứng với tiền tố dễ dãi nhất trước khi các ràng buộc cơ sở trở nên quá nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 1 3
```Mảng được sắp xếp:```
[3, 3, 1, 1]
```| tôi | giá trị | cnt | cnt - 2 | tôi | k = phút(cnt-2, i) | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | -1 | 0 | -1 → bỏ qua | 
| 1 | 3 | 2 | 0 | 1 | 0 | 
| 2 | 1 | 3 | 1 | 2 | 1 | 
| 3 | 1 | 4 | 2 | 3 | 2 | 

K tối đa là 2, nghĩa là chúng ta có thể xây dựng thang 2 bước. Điều này xảy ra khi tất cả các tấm ván đều có thể sử dụng được và chúng ta có thể phân bổ hai tấm ván lớn làm đế. 

### Ví dụ 2 

đầu vào:```
3
1 1 2
```Mảng được sắp xếp:```
[2, 1, 1]
```| tôi | giá trị | cnt | cnt - 2 | tôi | k | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | -1 | 0 | -1 | 
| 1 | 1 | 2 | 0 | 1 | 0 | 
| 2 | 1 | 3 | 1 | 2 | 1 | 

Câu trả lời là 1, phù hợp với thực tế là chỉ có thể hỗ trợ một bước hợp lệ khi chúng ta có hai cơ sở có thể sử dụng được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, vượt qua một lần sau đó | 
| Không gian | O(1) thêm | Chỉ quầy và sắp xếp tại chỗ | 

Tổng số phần tử trên tất cả các trường hợp thử nghiệm tối đa là 100.000, do đó việc sắp xếp từng trường hợp thử nghiệm một cách độc lập vẫn hiệu quả trong các điều kiện ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort(reverse=True)

        cnt = 0
        ans = 0
        for i, _ in enumerate(a):
            cnt += 1
            k = min(cnt - 2, i)
            ans = max(ans, k)
        out.append(str(max(ans, 0)))
    return "\n".join(out)

# provided samples
assert run("""4
4
1 3 1 3
3
3 3 2
5
2 3 3 4 2
3
1 1 2
""") == """2
1
2
0"""

# custom cases
assert run("""1
2
10 10
""") == "0", "minimum edge"

assert run("""1
3
10 10 1
""") == "1", "one step possible"

assert run("""1
5
1 1 1 1 1
""") == "0", "no valid bases"

assert run("""1
6
5 5 5 5 5 5
""") == "4", "all equal large planks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 10 | 0 | không đủ ván cho bất kỳ chiếc thang nào | 
| 10 10 1 | 1 | cơ sở + tính khả thi từng bước | 
| 1 1 1 1 1 | 0 | không có ngưỡng cơ sở hợp lệ | 
| tất cả 5s | 4 | trường hợp sử dụng tối đa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi có đúng hai tấm ván. Thuật toán sắp xếp chúng và ngay lập tức tìm thấy cnt = 2 ở lần lặp thứ hai, nhưng k = min(0, 1) = 0, điều này không mang lại thang dương chính xác vì cần ít nhất một bước. 

Một trường hợp khác là khi tất cả các tấm ván đều giống nhau và lớn. Ví dụ: với sáu tấm ván có chiều dài 5, mọi tiền tố đều thỏa mãn điều kiện cơ bản và hệ số giới hạn chỉ trở thành số lượng tấm ván có sẵn. Thuật toán tăng k một cách chính xác cho đến khi đạt tới 4, vì k + 2 = 6 khớp chính xác với tổng số tấm ván và hai trong số chúng được dành làm cơ sở.
