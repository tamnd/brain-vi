---
title: "CF 105859J - Đua xe leo đồi"
description: "Chúng ta có địa hình một chiều được mô tả bằng độ cao tại các điểm cách đều nhau. Xe di chuyển từ điểm ngoài cùng bên trái đến điểm ngoài cùng bên phải, bước từ chỉ số 0 đến chỉ số 1, rồi từ 1 đến 2, cứ như vậy cho đến chỉ số l. Mỗi bước tương ứng với việc di chuyển một mét theo chiều ngang."
date: "2026-06-25T14:42:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105859
codeforces_index: "J"
codeforces_contest_name: "Mines HSPC 2025 Open Division"
rating: 0
weight: 105859
solve_time_s: 37
verified: true
draft: false
---

[CF 105859J - Đua xe leo đồi](https://codeforces.com/problemset/problem/105859/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có địa hình một chiều được mô tả bằng độ cao tại các điểm cách đều nhau. Xe di chuyển từ điểm ngoài cùng bên trái đến điểm ngoài cùng bên phải, bước từ chỉ số 0 đến chỉ số 1, rồi từ 1 đến 2, cứ như vậy cho đến chỉ số l. Mỗi bước tương ứng với việc di chuyển một mét theo chiều ngang. 

Hạn chế chính là độ dốc của địa hình giữa các điểm liên tiếp. Nếu chiều cao tăng quá mạnh từ vị trí i lên i+1, xe có thể không đủ khả năng tăng tốc để leo lên đoạn đường đó. Về mặt hình thức, ô tô chỉ có thể xử lý những thay đổi về chiều cao lên đến giới hạn cố định cho mỗi bước. Nếu điểm tiếp theo thấp hơn, việc giảm dần luôn được phép bất kể độ lớn. 

Nhiệm vụ là xác định xem mọi cặp điểm liên tiếp có thể đi qua theo quy tắc này hay không. Nếu thậm chí một bước đi lên vượt quá mức tăng cho phép, toàn bộ lộ trình sẽ trở nên không thể thực hiện được. 

Kích thước đầu vào lên tới 10^6 vị trí, điều này buộc giải pháp phải hoàn toàn tuyến tính theo số lượng mẫu chiều cao. Bất kỳ giải pháp nào cố gắng tính toán lại đường dẫn, sử dụng so sánh lồng nhau hoặc mô phỏng chuyển động vật lý ngoài các lần kiểm tra liền kề đơn giản sẽ hết thời gian chờ. Cách tiếp cận khả thi duy nhất là một lần vượt qua mảng tính toán sự khác biệt cục bộ. 

Một trường hợp khó phát hiện khi độ cao dao động. Ví dụ: một chuỗi như 0 5 1 5 0 với chữ a nhỏ có vẻ có thể quản lý được trên tổng thể nhưng chỉ một bước nhảy lên trên mới xác định thất bại. Một trường hợp khác là khi tất cả các mức tăng riêng lẻ đều nhỏ nhưng trực giác tích lũy có thể đánh lừa: không có sự tích lũy, chỉ có các ràng buộc theo từng bước. 

Một trường hợp góc khác là địa hình giảm dần, chẳng hạn như 10 1 0 0 0. Ngay cả với gia tốc rất nhỏ, điều này luôn đúng vì chuyển động đi xuống không gây ra hạn chế nào. 

## Phương pháp tiếp cận 

Tư duy bạo lực sẽ mô phỏng quá trình di chuyển của ô tô và với mỗi bước, hãy kiểm tra xem liệu ô tô có thể leo từ điểm hiện tại sang điểm tiếp theo hay không. Theo nghĩa đen nhất, điều này vẫn chỉ yêu cầu quét các cặp liền kề một lần, nhưng người ta có thể cho rằng không chính xác rằng cần có lý luận phức tạp hơn, chẳng hạn như theo dõi năng lượng, duy trì cấu hình độ dốc hoặc xem xét các đoạn dài hơn. Những cách giải thích đó nhanh chóng trở nên không cần thiết vì quy tắc này hoàn toàn mang tính cục bộ: chỉ h[i+1] - h[i] quan trọng khi dương. 

Hướng không chính xác ngây thơ sẽ là thử tính toán độ dốc tối đa trên các cửa sổ lớn hơn hoặc cố gắng làm phẳng địa hình, điều này sẽ dẫn đến hành vi O(l^2) hoặc tệ hơn nếu được thực hiện theo nghĩa đen. 

Nhận xét đúng là tính khả thi được xác định độc lập ở mỗi bước. Ô tô chỉ bị hỏng khi gặp một chuyển tiếp duy nhất trong đó bước nhảy lên vượt quá a. Điều này làm giảm toàn bộ vấn đề thành việc tìm xem liệu chênh lệch dương tối đa giữa các phần tử liên tiếp có nhiều nhất là a hay không. 

Do đó, chúng tôi tính toán tất cả các khác biệt liền kề và theo dõi độ dốc hướng lên tối đa. Nếu bất kỳ ai vượt quá a, câu trả lời sẽ có ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra tất cả các cặp liền kề | O(l) | O(1) | Đã chấp nhận | 
| Bất kỳ phương pháp toàn cầu hoặc dựa trên cửa sổ nào | O(l^2) hoặc O(l log l) | O(l) | Quá chậm/không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc l và a, sau đó đọc mảng h có độ dài l+1 biểu thị độ cao dọc theo đường đi. 
2. Khởi tạo biến max_up bằng 0 để lưu trữ bước nhảy lên lớn nhất được thấy cho đến nay. 
3. Lặp lại i từ 0 đến l-1, kiểm tra từng cặp liên tiếp h[i] và h[i+1]. 
4. Tính hiệu h[i+1] - h[i]. Nếu giá trị này dương, hãy so sánh nó với max_up và cập nhật max_up nếu nó lớn hơn. 
5. Sau khi xử lý tất cả các cặp, so sánh max_up với a. Nếu max_up lớn hơn a, xuất ra "BÁO CÁO LỖI". 
6. Ngược lại, xuất ra "CÓ THỂ".

Lý do điều này có tác dụng là vì mỗi lần di chuyển hợp lệ chỉ phụ thuộc vào việc mỗi lần đi lên cục bộ có khả thi hay không. Các động thái đi xuống không áp đặt ràng buộc nào và có thể bị bỏ qua hoàn toàn. Thuật toán giảm địa hình xuống mức tồi tệ nhất và tính khả thi chỉ phụ thuộc vào giá trị cực trị duy nhất đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    l, a = map(int, input().split())
    h = list(map(int, input().split()))

    max_up = 0

    for i in range(l):
        diff = h[i + 1] - h[i]
        if diff > 0:
            if diff > max_up:
                max_up = diff

    if max_up > a:
        print("BUG REPORT")
    else:
        print("POSSIBLE")

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của quan sát rằng chỉ có sự khác biệt trở lên mới quan trọng. Vòng lặp chạy chính xác l lần, so sánh các phần tử liền kề một lần. Biến max_up nắm bắt số liệu thống kê quan trọng duy nhất quyết định thành công. 

Một sai lầm phổ biến ở đây là lấy sự khác biệt tuyệt đối. Điều đó sẽ xử phạt việc di chuyển xuống dốc một cách không chính xác, điều mà vấn đề cho phép một cách rõ ràng mà không bị hạn chế. Một sai lầm khác là sử dụng tiền tố cực đại hoặc cố gắng “mô phỏng” chuyển động của xe ngoài các so sánh liền kề, điều này gây ra sự phức tạp không cần thiết mà không ảnh hưởng đến độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 2 5 1 1
```| tôi | h[i] | h[i+1] | khác biệt | max_up | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | 1 | 
| 1 | 2 | 5 | 3 | 3 | 
| 2 | 5 | 1 | -4 | 3 | 
| 3 | 1 | 1 | 0 | 3 | 

Bước nhảy lên lớn nhất là 3, khớp với a. Đường dẫn chính xác ở giới hạn nên nó hợp lệ và đầu ra CÓ THỂ. 

### Ví dụ 2 

đầu vào:```
4 3
0 3 1 5 0
```| tôi | h[i] | h[i+1] | khác biệt | max_up | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 3 | 3 | 
| 1 | 3 | 1 | -2 | 3 | 
| 2 | 1 | 5 | 4 | 4 | 
| 3 | 5 | 0 | -5 | 4 | 

Ở đây, bước tăng tối đa là 4, vượt quá a = 3. Xe bị kẹt khi chuyển từ 1 sang 5, vì vậy đầu ra chính xác là BÁO CÁO LỖI. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(l) | Mỗi cặp liền kề được kiểm tra một lần trong một lần duyệt qua mảng | 
| Không gian | O(1) | Chỉ có một biến duy nhất theo dõi độ dốc hướng lên tối đa | 

Các ràng buộc cho phép tối đa 10^6 giá trị chiều cao, do đó cần phải quét tuyến tính. Bất kỳ thuật toán nào yêu cầu sắp xếp hoặc truyền tải lồng nhau sẽ quá chậm, nhưng giải pháp này chỉ thực hiện một lượng công việc không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4 3\n1 2 5 1 1\n") == "POSSIBLE"
assert run("4 3\n0 3 1 5 0\n") == "BUG REPORT"
assert run("3 1000000\n0 999999 1 1000000\n") == "POSSIBLE"

# minimum size (single step)
assert run("1 5\n0 10\n") == "BUG REPORT"

# all equal heights
assert run("3 1\n5 5 5 5\n") == "POSSIBLE"

# strictly decreasing
assert run("4 1\n10 7 3 1 0\n") == "POSSIBLE"

# tight boundary case
assert run("2 2\n1 3 1\n") == "POSSIBLE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mức tăng duy nhất vượt quá giới hạn | BÁO CÁO LỖI | trường hợp thất bại tối thiểu | 
| địa hình bằng phẳng | CÓ THỂ | không có ràng buộc hướng lên nào được kích hoạt | 
| dãy giảm dần | CÓ THỂ | xuống dốc không hạn chế | 
| bình đẳng ranh giới chính xác | CÓ THỂ | được phép bình đẳng | 

## Vỏ cạnh 

Địa hình một bước như`0 10`ngay lập tức tiết lộ liệu việc triển khai có chính xác chỉ tập trung vào những khác biệt liền kề hay không. Thuật toán tính một bước nhảy lên 10 và so sánh nó với a. 

Địa hình bằng phẳng như`5 5 5 5`chỉ tạo ra sự khác biệt bằng không. max_up vẫn bằng 0, do đó mọi gia tốc không âm đều vượt qua. 

Một chuỗi giảm dần nghiêm ngặt như`10 7 3 1 0`kiểm tra rằng sự khác biệt tiêu cực được bỏ qua hoàn toàn. Ngay cả khi địa hình dốc xuống, không có ràng buộc nào bị vi phạm. 

Một trường hợp đẳng thức biên như`1 3 1`với a = 2 xác nhận rằng điều kiện đã bao hàm. Thuật toán ghi lại max_up = 2 và chấp nhận đường dẫn một cách chính xác mà không bác bỏ đẳng thức.
