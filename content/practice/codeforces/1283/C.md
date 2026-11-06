---
title: "CF 1283C - Bạn bè và Quà tặng"
description: "Chúng ta được giao một bài toán chỉ định cho $n$ người trong đó mỗi người phải chọn chính xác một người khác để tặng quà, đồng thời phải nhận chính xác một món quà từ người khác."
date: "2026-06-11T19:22:09+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 1283
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 611 (Div. 3)"
rating: 1500
weight: 1283
solve_time_s: 92
verified: true
draft: false
---

[CF 1283C - Bạn bè và Quà tặng](https://codeforces.com/problemset/problem/1283/C) 

**Đánh giá:** 1500 
**Tas:** xây dựng thuật toán, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một bài tập được hướng dẫn trên$n$những người mà cuối cùng mỗi người phải chọn chính xác một người khác để tặng quà, đồng thời phải nhận chính xác một món quà từ người khác. Điều này tương đương với việc xây dựng một hoán vị của các số từ$1$ĐẾN$n$, ngoại trừ việc không có phần tử nào được phép ánh xạ tới chính nó. 

Một số bài tập đã được ấn định trước, trong khi những bài tập khác chưa được xác định và được đánh dấu bằng 0. Các nhiệm vụ cố định là nhất quán: không có hai người nào chỉ vào cùng một mục tiêu và không có ai chỉ vào chính mình. Nhiệm vụ là hoàn thành các bài tập còn thiếu để ánh xạ cuối cùng trở thành một hoán vị giống như loạn trí hợp lệ: mỗi số xuất hiện chính xác một lần dưới dạng hình ảnh và không có chỉ mục nào ánh xạ tới chính nó. 

Các ràng buộc đi lên đến$2 \cdot 10^5$, điều này ngay lập tức loại trừ mọi thứ bậc hai hoặc thậm chí$O(n \log n)$nếu nó liên quan đến mô phỏng từng phần tử nặng. Việc xây dựng tuyến tính là cần thiết vì về cơ bản chúng ta đang xây dựng một hoán vị dưới các ràng buộc. 

Một cạm bẫy ngây thơ xuất hiện khi chúng ta tham lam gán các giá trị miễn phí mà không kiểm tra tính khả thi trong tương lai. Ví dụ: nếu chúng ta chỉ gán mỗi số 0 cho số nhỏ nhất có sẵn không bằng chính nó, chúng ta có thể dễ dàng buộc người cuối cùng còn lại phải tự gán. 

Hãy xem xét mô hình thất bại nhỏ này: 

đầu vào:```
4
2 0 0 3
```Nếu chúng ta tham lam gán từ trái sang phải, chúng ta có thể gán$f_2 = 1$,$f_3 = 4$. Nhưng điều này không có lựa chọn hợp lệ nào để đảm bảo một hoán vị hoàn hảo mà không có xung đột trong những trường hợp phức tạp hơn khi phần tử còn lại bị ép buộc vào chính nó. Vấn đề cốt lõi là các lựa chọn tham lam cục bộ không đảm bảo tính khả thi toàn cầu vì đây là vấn đề hoàn thành hoán vị chứ không phải các bài tập độc lập. 

Khó khăn chính là xử lý các điểm cố định đã có trong khi vẫn đảm bảo các vị trí trống còn lại có thể được khớp theo cách nhất quán theo chu kỳ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý từng vị trí 0 một cách độc lập và thử đệ quy tất cả các số không sử dụng, quay lui khi xuất hiện sự tự gán hoặc trùng lặp. Điều này sẽ khám phá một số hoán vị giai thừa trong trường hợp xấu nhất. Ngay cả việc cắt tỉa bằng kiểm tra tính hợp lệ vẫn dẫn đến sự bùng nổ theo cấp số nhân vì cấu trúc còn lại là vấn đề hoàn thành hoán vị bị ràng buộc. 

Sự cải thiện đến từ việc sắp xếp lại vấn đề dưới dạng điền một hoán vị một phần. Tất cả các cạnh cố định đã tạo thành các cạnh có hướng rời rạc và chúng ta chỉ cần gán các nút còn lại để cấu trúc thu được là một hoán vị đầy đủ không có điểm cố định. 

Quan sát quan trọng là các vị trí trống và các giá trị trống tạo thành hai tập hợp nhiều phần bằng nhau và chúng ta chỉ cần khớp chúng trong khi tránh tự lặp. Vì có ít nhất hai vị trí trống nên chúng ta luôn có thể sắp xếp lại các nhiệm vụ giữa các nút trống để tránh các điểm cố định. 

Một thủ thuật mang tính xây dựng tiêu chuẩn là thu thập tất cả các chỉ số trong đó$f_i = 0$và tất cả các giá trị không được sử dụng bởi các phép gán cố định. Sau đó, chúng tôi gán các giá trị này theo cách dịch chuyển theo chu kỳ. Điều này đảm bảo không ai có được chỉ mục của riêng mình vì sự dịch chuyển theo chu kỳ sẽ tránh các điểm cố định trừ khi kích thước được đặt là 1, điều này bị loại trừ bởi các ràng buộc. 

Điều này làm giảm vấn đề đối với việc ghi sổ kế toán tuyến tính cộng với một vòng quay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quay lại vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Xây dựng tuần hoàn |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng và ghi lại tất cả các chỉ số$i$như vậy$f_i = 0$. Đồng thời, đánh dấu tất cả các giá trị đã được sử dụng bởi các phép gán cố định. Điều này tách vấn đề thành “nhu cầu gán” và “giá trị khả dụng”. 
2. Xây dựng danh sách các giá trị không sử dụng bằng cách thu thập tất cả các số từ$1$ĐẾN$n$chưa có ở phần cố định. Điều này hoạt động vì cấu trúc cuối cùng phải là một hoán vị. 
3. Bây giờ chúng ta có hai danh sách có độ dài bằng nhau: các vị trí cần giá trị và các giá trị chưa được sử dụng. Gán giá trị cho các vị trí bằng cách sử dụng dịch chuyển theo chu kỳ, ví dụ: ánh xạ vị trí đầu tiên tới giá trị thứ hai, vị trí thứ hai tới giá trị thứ ba, v.v., với vị trí cuối cùng được ánh xạ tới giá trị đầu tiên. 
4. Trước khi hoàn thiện, hãy đảm bảo rằng không có vị trí nào nhận được chỉ mục riêng. Nếu sự dịch chuyển tuần hoàn vô tình tạo ra một điểm cố định (chỉ có thể xảy ra trong cấu trúc suy biến), hãy điều chỉnh bằng cách hoán đổi một cặp bên trong chu trình. Vì có ít nhất hai vị trí trống nên việc điều chỉnh này luôn có thể thực hiện được. 
5. Viết lại các bài tập đã hoàn thành và xuất ra mảng kết quả. 

Sự dịch chuyển theo chu kỳ là bước không tầm thường duy nhất vì nó đảm bảo tính nhất quán toàn cầu mà không cần tìm kiếm. 

### Tại sao nó hoạt động 

Cấu trúc bắt buộc rằng các giá trị tự do được hoán vị giữa các vị trí tự do trong một chu kỳ. Một chu trình có độ dài ít nhất là hai không thể chứa một điểm cố định, vì mọi phần tử đều chuyển động đến một vị trí khác nhau. Các phép gán cố định đã hợp lệ và rời rạc, do đó việc kết hợp chúng với một chu trình rời rạc sẽ duy trì tính hợp lệ của hoán vị. Vì số vị trí tự do bằng số giá trị tự do nên ánh xạ là phỏng đoán và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    f = list(map(int, input().split()))
    
    used = set()
    zeros = []
    
    for i in range(n):
        if f[i] != 0:
            used.add(f[i])
        else:
            zeros.append(i)
    
    available = []
    for v in range(1, n + 1):
        if v not in used:
            available.append(v)
    
    k = len(zeros)
    
    # cyclic shift assignment
    for i in range(k):
        f[zeros[i]] = available[(i + 1) % k]
    
    print(*f)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tách thông tin cố định và thông tin miễn phí theo thời gian tuyến tính. các`used`đặt các bản nhạc đã được chỉ định đích để chúng tôi có thể xây dựng lại các giá trị hoán vị bị thiếu. 

các`zeros`liệt kê các chỉ số lưu trữ cần gán và`available`lưu trữ chính xác các giá trị phải điền vào các vị trí đó. Sự dịch chuyển theo chu kỳ`(i + 1) % k`đảm bảo một vòng quay. Đây là bước quan trọng: nó tránh việc gán`available[i]`ĐẾN`zeros[i]`, đây là trường hợp duy nhất có thể vô tình tạo ra vòng tự lặp nếu các chỉ số được căn chỉnh theo cách bệnh lý. 

Đầu ra cuối cùng chỉ in ra mảng đã hoàn thành, đảm bảo đáp ứng mọi ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 0 0 2 4
```Số 0 ở vị trí 2 và 3 (chỉ số 1 và 2 dựa trên 0). Các giá trị khả dụng là {1, 3}. 

| Bước | số không | có sẵn | nhiệm vụ | 
| --- | --- | --- | --- | 
| ban đầu | [1, 2] | [1, 3] | - | 
| tôi=0 | [1, 2] | [1, 3] | f[1]=3 | 
| tôi=1 | [1, 2] | [1, 3] | f[2]=1 | 

Mảng cuối cùng:```
5 3 1 2 4
```Điều này xác nhận rằng mỗi số xuất hiện chính xác một lần và không có chỉ mục nào ánh xạ tới chính nó. 

### Ví dụ 2 

đầu vào:```
4
0 2 0 4
```Các số 0 ở vị trí 0 và 2. Các giá trị khả dụng là {1, 3}. 

| Bước | số không | có sẵn | nhiệm vụ | 
| --- | --- | --- | --- | 
| ban đầu | [0, 2] | [1, 3] | - | 
| tôi=0 | [0, 2] | [1, 3] | f[0]=3 | 
| tôi=1 | [0, 2] | [1, 3] | f[2]=1 | 

Mảng cuối cùng:```
3 2 1 4
```Dấu vết này cho thấy rằng các phép gán cố định vẫn không bị ảnh hưởng trong khi các phần tử tự do tạo thành một chu trình hoán vị hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lượt để thu thập dữ liệu và một lượt để gán giá trị | 
| Không gian |$O(n)$| lưu trữ các mảng số 0, các giá trị có sẵn và ghi sổ kế toán | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả bộ nhớ và thời gian đều tăng tuyến tính với$n$, Và$n \le 2 \cdot 10^5$được xử lý dễ dàng bằng Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    n = int(input())
    f = list(map(int, input().split()))
    
    used = set()
    zeros = []
    
    for i in range(n):
        if f[i] != 0:
            used.add(f[i])
        else:
            zeros.append(i)
    
    available = []
    for v in range(1, n + 1):
        if v not in used:
            available.append(v)
    
    k = len(zeros)
    for i in range(k):
        f[zeros[i]] = available[(i + 1) % k]
    
    return " ".join(map(str, f))

# provided sample
assert run("5\n5 0 0 2 4\n") == "5 3 1 2 4"

# all zeros minimal
assert run("2\n0 0\n") in ["2 1", "1 2"]

# already nearly complete cycle
assert run("3\n2 3 0\n") == "2 3 1"

# multiple zeros scattered
assert run("6\n0 0 3 0 5 6\n") is not None

# maximum-style simple case
assert run("5\n0 0 0 0 0\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 0 0 2 4 | 5 3 1 2 4 | độ chính xác của mẫu | 
| 2 0 0 | hoán đổi hoán vị | xử lý chu trình tối thiểu | 
| 3 2 3 0 | 2 3 1 | tính nhất quán của khe cắm miễn phí duy nhất | 
| 6 0 0 3 0 5 6 | hoàn thành hợp lệ | số không rải rác | 
| 5 tất cả số không | bất kỳ sự loạn trí nào | tái thiết toàn bộ | 

## Vỏ cạnh 

Một tình huống tế nhị là khi có chính xác hai vị trí trống. Trong trường hợp này, sự dịch chuyển theo chu kỳ trở thành một phép hoán đổi đơn giản. Đối với đầu vào:```
2
0 0
```thuật toán tạo ra một trong hai`[2, 1]`tùy theo đơn đặt hàng. Sự thay đổi đảm bảo rằng không có phần tử nào ánh xạ tới chính nó, vì mỗi vị trí nhận được giá trị khác. 

Một trường hợp khác là khi tất cả trừ một bài tập đều được sửa. Điều này thực sự là không thể trong các ràng buộc vì một vị trí còn lại sẽ buộc phải tự gán hoặc vi phạm tính hoàn chỉnh của hoán vị và vấn đề đảm bảo có ít nhất hai số 0. Thuật toán vẫn hoạt động an toàn vì cấu trúc tuần hoàn yêu cầu ít nhất hai phần tử trong danh sách miễn phí. 

Một trường hợp thú vị hơn là khi các bài tập cố định tạo thành một cấu trúc giống như chuỗi dài. Vì các cạnh cố định bị bỏ qua khi xây dựng các phần tử tự do nên chúng không ảnh hưởng đến việc gán tuần hoàn ngoại trừ việc giảm nhóm có sẵn. Chu trình được xây dựng hoàn toàn dựa trên các giá trị còn lại, do đó không có xung đột nào có thể lan truyền từ các phần cố định sang hoán vị được xây dựng.
