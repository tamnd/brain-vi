---
title: "CF 105873E - Hộp Elisas"
description: "Chúng ta được cho một dãy hộp, mỗi hộp có sức chứa cố định. Ngoài ra, còn có một con số biểu thị số lượng hiện vật giống hệt nhau mà Elisa muốn đặt vào trong một chiếc hộp."
date: "2026-06-25T14:26:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105873
codeforces_index: "E"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105873
solve_time_s: 39
verified: true
draft: false
---

[CF 105873E - Hộp Elisas](https://codeforces.com/problemset/problem/105873/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy hộp, mỗi hộp có sức chứa cố định. Ngoài ra, còn có một con số biểu thị số lượng hiện vật giống hệt nhau mà Elisa muốn đặt vào trong một chiếc hộp. Nhiệm vụ là chọn một chiếc hộp có thể chứa tất cả hiện vật cùng một lúc, nghĩa là sức chứa của nó ít nhất phải bằng số lượng yêu cầu. Nếu có nhiều hộp thỏa mãn điều kiện này thì hộp được chọn phải là hộp sớm nhất trong hàng. Nếu không có ô nào đủ lớn thì câu trả lời là không có lựa chọn hợp lệ nào. 

Đầu vào về cơ bản là danh sách dung lượng cộng với giá trị ngưỡng. Đầu ra là một chỉ số duy nhất trỏ đến dung lượng đầu tiên không nhỏ hơn ngưỡng. 

Các ràng buộc cho phép lên tới một trăm nghìn hộp và dung lượng cũng có thể lớn. Điều này ngay lập tức loại trừ mọi thứ bậc hai như kiểm tra tất cả các cặp hoặc quét liên tục các phân đoạn cho mỗi truy vấn. Một lần vượt qua tuyến tính đã nằm trong giới hạn thoải mái, vì một lần vượt qua một trăm nghìn phần tử là không đáng kể trong các giới hạn thời gian thông thường. 

Các trường hợp thất bại tinh vi chủ yếu liên quan đến tính đúng đắn hơn là hiệu suất. Một lỗi phổ biến là trả về bất kỳ chỉ mục hợp lệ nào thay vì chỉ mục nhỏ nhất. Ví dụ, nếu năng lực là`5 1 10 7`và yêu cầu là`6`, cả chỉ mục 3 và 4 đều hoạt động nhưng phải chọn chỉ mục 3. Một sai lầm nữa là quên trường hợp không có hộp nào hoạt động, chẳng hạn như dung lượng`1 2 3`với yêu cầu`10`, nơi đầu ra chính xác là`-1`thay vì một chỉ mục không hợp lệ như`0`hoặc`N`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi quét từng hộp và kiểm tra xem dung lượng của nó ít nhất có đạt yêu cầu hay không. Chúng tôi theo dõi tất cả các chỉ số hợp lệ hoặc chỉ ghi nhớ chỉ số hợp lệ đầu tiên. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của câu trả lời. Chi phí của nó là một so sánh cho mỗi hộp, dẫn đến hoạt động O(N), vốn đã đủ hiệu quả cho các giới hạn nhất định. 

Không cần các kỹ thuật tối ưu hóa phức tạp hơn như tìm kiếm nhị phân hoặc tiền xử lý tiền tố vì chỉ có một truy vấn và không có cấu trúc nào ngoài một thứ tự tuyến tính duy nhất. Tối ưu hóa có ý nghĩa duy nhất là dừng sớm khi tìm thấy ô hợp lệ đầu tiên, vì các ứng viên sau này không thể cải thiện câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force | O(N) | O(1) | Đã chấp nhận | 
| Dừng quét sớm | O(N) trường hợp xấu nhất, O(k) trường hợp tốt nhất | O(1) | Đã chấp nhận | 

Quan sát chính là yêu cầu đơn điệu theo thứ tự chỉ mục chỉ về mặt quét chứ không theo thứ tự giá trị, do đó việc sắp xếp hoặc cấu trúc dữ liệu nâng cao sẽ chỉ phá hủy yêu cầu lập chỉ mục ban đầu. 

## Hướng dẫn thuật toán 

1. Đọc số lượng hộp và giá trị dung lượng yêu cầu. 
2. Đọc danh sách năng lực theo thứ tự đã cho. 
3. Duyệt danh sách từ trái sang phải, kiểm tra từng năng lực theo yêu cầu. 
4. Ngay khi tìm thấy dung lượng ít nhất bằng giá trị yêu cầu, hãy xuất chỉ mục của nó và kết thúc. 
5. Nếu quá trình duyệt kết thúc mà không tìm thấy bất kỳ hộp hợp lệ nào, hãy xuất`-1`. 

Lý do dừng ngay lập tức ở ô hợp lệ đầu tiên là vì các chỉ số tăng lên khi chúng ta di chuyển sang phải và bài toán rõ ràng ưu tiên chỉ số nhỏ nhất trong số tất cả các lựa chọn hợp lệ. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là câu trả lời được xác định hoàn toàn bằng một vị từ trên mỗi chỉ mục một cách độc lập: một hộp có giá trị hoặc không chỉ phụ thuộc vào dung lượng của nó. Không có sự tương tác giữa các hộp, không tích lũy và không phụ thuộc vào các phần tử đã chọn trước đó. Do tính độc lập này, việc quét từ trái sang phải đảm bảo rằng ngay lần đầu tiên điều kiện được thỏa mãn, chúng ta đã đạt được chỉ số nhỏ nhất có thể thỏa mãn nó. Không có quyết định nào sau này có thể thay thế nó bằng một ứng cử viên tốt hơn vì bất kỳ chỉ số nào sau này đều lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    for i, val in enumerate(a, 1):
        if val >= m:
            print(i)
            return
    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào theo thời gian tuyến tính và quét ngay danh sách. Việc liệt kê bắt đầu từ 1 vì đầu ra là lập chỉ mục dựa trên 1. Việc quay lại sớm đảm bảo rằng chúng tôi không tiếp tục quét sau khi tìm thấy câu trả lời tối ưu, mặc dù ngay cả khi không có nó thì độ phức tạp sẽ vẫn tuyến tính. 

Một lỗi triển khai thường gặp là quên yêu cầu lập chỉ mục dựa trên 1, khiến mọi câu trả lời đều thay đổi một. Một cách khác là sử dụng so sánh nghiêm ngặt`>`thay vì`>=`, từ chối không chính xác các hộp có dung lượng khớp chính xác với yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 10
15 20 10 12 10
```Chúng tôi theo dõi quá trình quét: 

| Chỉ mục | Công suất | Tình trạng (>=10) | Hành động | 
| --- | --- | --- | --- | 
| 1 | 15 | đúng | dừng lại | 

Ô đầu tiên đã thỏa mãn điều kiện nên ta trả về 1 ngay. Điều này cho thấy tại sao việc chấm dứt sớm là hợp lý và tối ưu. 

### Ví dụ 2 

đầu vào:```
5 100
15 20 10 12 10
```| Chỉ mục | Công suất | Tình trạng (>=100) | Hành động | 
| --- | --- | --- | --- | 
| 1 | 15 | sai | tiếp tục | 
| 2 | 20 | sai | tiếp tục | 
| 3 | 10 | sai | tiếp tục | 
| 4 | 12 | sai | tiếp tục | 
| 5 | 10 | sai | kết thúc | 

Không có hộp nào thỏa mãn yêu cầu nên ta xuất`-1`. Điều này xác nhận việc xử lý đúng trường hợp giải pháp trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi hộp được kiểm tra một lần, với công việc liên tục cho mỗi lần kiểm tra | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng ngoài mảng đầu vào | 

Quá trình quét tuyến tính dễ dàng phù hợp với giới hạn lên tới 100.000 hộp vì nó chỉ thực hiện các phép so sánh đơn giản và dừng sớm trong các trường hợp điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp))

# Re-define solve with return for testing
def solve_output(inp: str):
    data = inp.strip().split()
    n, m = map(int, data[:2])
    a = list(map(int, data[2:2+n]))
    for i, v in enumerate(a, 1):
        if v >= m:
            return i
    return -1

# provided samples
assert solve_output("5 10\n15 20 10 12 10") == 1
assert solve_output("5 100\n15 20 10 12 10") == -1

# custom cases
assert solve_output("1 1\n1") == 1, "single valid"
assert solve_output("1 2\n1") == -1, "single invalid"
assert solve_output("5 5\n1 2 3 4 5") == 5, "last element"
assert solve_output("5 5\n5 5 5 5 5") == 1, "first match among equals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn hợp lệ | 1 | chấp nhận kích thước tối thiểu | 
| phần tử đơn không hợp lệ | -1 | từ chối kích thước tối thiểu | 
| mảng tăng dần | chỉ mục cuối cùng | đảm bảo tính chính xác của quá trình quét toàn bộ | 
| tất cả đều có giá trị như nhau | 1 | ưu tiên chỉ số sớm nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hộp đầu tiên đã đáp ứng được yêu cầu. Thuật toán xử lý nó bằng cách trả về ngay chỉ mục 1 mà không cần quét thêm, vì lần so sánh đầu tiên thành công và kích hoạt việc chấm dứt. 

Một trường hợp khác là không có ô nào thỏa mãn yêu cầu. Vòng lặp hoàn thành đầy đủ và hàm quay trở lại trạng thái trả về`-1`, chỉ ra chính xác sự vắng mặt của giải pháp. Đối với đầu vào như`3 10`với năng lực`1 2 3`, mọi so sánh đều thất bại, do đó không xảy ra sự quay lại sớm và kết quả cuối cùng là`-1`. 

Trường hợp thứ ba là khi có nhiều hộp thỏa mãn yêu cầu. Vì`5 5`với năng lực`5 7 6 5`, thuật toán trả về chỉ số 1 vì đây là lần đầu tiên điều kiện trở thành đúng. Các hộp hợp lệ sau này không bao giờ được xem xét, phù hợp với yêu cầu chọn chỉ mục nhỏ nhất.
