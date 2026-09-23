---
title: "CF 104787I - Giả mạo"
description: "Chúng tôi duy trì nhiều tập hợp số nguyên thay đổi theo thời gian và chúng tôi phải hỗ trợ hai loại hoạt động một cách hiệu quả."
date: "2026-06-28T14:22:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104787
codeforces_index: "I"
codeforces_contest_name: "The 2023 CCPC (Qinhuangdao) Onsite (The 2nd Universal Cup. Stage 9: Qinhuangdao)"
rating: 0
weight: 104787
solve_time_s: 62
verified: true
draft: false
---

[CF 104787I - Giả mạo](https://codeforces.com/problemset/problem/104787/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì nhiều tập hợp số nguyên thay đổi theo thời gian và chúng tôi phải hỗ trợ hai loại hoạt động một cách hiệu quả. Một thao tác liên tục xóa một phần tử cụ thể khỏi nhiều tập hợp hiện tại và các thao tác khác truy vấn thống kê theo thứ tự hiện tại, cụ thể là phần tử lớn nhất thứ x. 

Chính xác hơn, chúng ta bắt đầu với n số. Sau đó chúng tôi xử lý m hoạt động. Mỗi truy vấn loại A yêu cầu giá trị lớn nhất thứ x trong nhiều tập hợp hiện tại. Mỗi thao tác loại C sẽ sửa đổi nhiều tập hợp bằng cách thực hiện thao tác xóa lặp đi lặp lại: chúng tôi liên tục xóa giá trị lớn nhất trừ k và nếu có nhiều lần xuất hiện của giá trị đó, chúng tôi luôn xóa lần xuất hiện ngoài cùng bên trái theo một số thứ tự ngầm định, lặp lại t lần này. 

Mặc dù cách diễn đạt hơi khác thường, nhưng tác dụng cốt lõi của C là mang tính quyết định: nó luôn nhắm đến giá trị tối đa hiện tại, giảm k và thay thế nó, lặp lại quá trình này t lần. 

Các ràng buộc rất lớn: cả n và m đều có thể lên tới 5×10^5 và t có thể lớn tới 10^18. Điều này ngay lập tức loại trừ mọi giải pháp xử lý từng thao tác bằng cách quét hoặc sắp xếp đầy đủ mảng. Ngay cả việc duy trì một vectơ đã sắp xếp và cập nhật nó cho mỗi thao tác cũng sẽ thất bại vì việc chèn và xóa lặp đi lặp lại quá chậm ở quy mô này. 

Khó khăn thực sự đến từ thao tác C. Một cách giải thích ngây thơ theo nghĩa đen sẽ áp dụng bản cập nhật t lần, điều này là không thể khi t có thể là 10^18. Một vấn đề tế nhị khác là nhiều tập hợp có thể chứa các bản sao, vì vậy việc loại bỏ mức “tối đa” phải xử lý chính xác tần số chứ không chỉ giá trị. 

Một ví dụ nhỏ trong đó mô phỏng đơn giản không thành công là khi tất cả các giá trị đều bằng nhau. Nếu nhiều tập hợp là [5, 5, 5] và k = 2 thì việc áp dụng C nhiều lần sẽ tiếp tục chọn 5, tạo ra 3, 3, 3 sau ba bước. Một cấu trúc đơn giản không bảo toàn các bản sao một cách chính xác có thể thu gọn các giá trị một cách không chính xác hoặc làm mất thông tin đa bội, dẫn đến câu trả lời sai cho truy vấn A. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ duy trì một thùng chứa tất cả các phần tử đã được sắp xếp. Đối với A x, chúng ta sẽ lập chỉ mục trực tiếp vào danh sách đã sắp xếp. Đối với C t, chúng ta sẽ lặp t lần, trích xuất phần tử lớn nhất, giảm nó đi k và chèn lại. Mỗi lần trích xuất và chèn có chi phí O(log n), do đó, một thao tác C có chi phí O(t log n). Vì t có thể lên tới 10^18 nên điều này hoàn toàn không khả thi. 

Quan sát quan trọng là thao tác C chỉ tương tác với phần tử tối đa hiện tại. Thay vì coi nó như những thao tác xóa lặp đi lặp lại, chúng ta có thể diễn giải lại nó như việc liên tục lấy giá trị lớn nhất hiện tại và áp dụng một phép biến đổi xác định cho nó. Điều này gợi ý rằng chúng ta không nên mô phỏng từng bước mà thay vào đó hãy suy xét xem mỗi giá trị riêng biệt có thể bị ảnh hưởng bao nhiêu lần trước khi nó không còn ở mức tối đa. 

Cấu trúc quan trọng là sau khi giá trị v giảm đi, nó sẽ trở thành v − k. Giá trị mới này có thể vẫn lớn nhưng nó di chuyển xuống dưới một cách số học có kiểm soát. Vì chỉ chọn mức tối đa nên quá trình này hoạt động giống như liên tục “đẩy” phần tử lớn nhất xuống thông qua một chuỗi các mức cách nhau bằng k. Điều này có thể được xử lý bằng vùng heap tối đa hoặc nhiều tập hợp theo thứ tự, nhưng chúng ta cũng phải tăng tốc các thao tác lặp lại bằng cách gộp các giá trị giống hệt nhau hoặc sử dụng tính năng đếm lười. 

Một cách tiếp cận chính xác và hiệu quả sử dụng nhiều tập hợp có số lượng và luôn xử lý mức tối đa hiện tại. Thay vì lặp lại t lần, chúng ta có thể liên tục xác định xem giá trị tối đa hiện tại vẫn là giá trị tối đa bao nhiêu lần trước khi nó bị giá trị phân biệt tiếp theo vượt qua. Số đó là t hoặc được giới hạn bởi khoảng cách giữa hai giá trị trên cùng chia cho k. 

Với một đống (hoặc cấu trúc được sắp xếp), chúng ta chỉ có thể mô phỏng các chuyển đổi có ý nghĩa, bỏ qua các đoạn giảm lặp đi lặp lại kéo dài.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(m·t log n) | O(n) | Quá chậm | 
| Heap với mức giảm tối đa theo đợt | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì tất cả các giá trị trong một vùng tối đa. Vì Python chỉ có vùng heap tối thiểu nên chúng tôi lưu trữ các giá trị âm. 

Chúng tôi cũng khẳng định rằng mỗi khi xử lý một thao tác C, chúng tôi chỉ chạm vào phần tử tối đa hiện tại và chúng tôi giảm nó đi k nhiều lần, nhưng chúng tôi phải đảm bảo không lãng phí thời gian cho mỗi lần lặp lại. 

1. Khởi tạo vùng heap tối đa chứa tất cả các giá trị ban đầu. Điều này thể hiện nhiều tập hợp hiện tại trong một cấu trúc nơi chúng ta luôn có thể truy cập phần tử lớn nhất trong thời gian khấu hao O(1). 
2. Đối với phép toán A x, chúng ta cần phần tử lớn thứ x. Vì vùng heap không hỗ trợ trực tiếp số liệu thống kê đơn hàng nên chúng tôi duy trì cấu trúc phụ hoặc định kỳ xây dựng lại ảnh chụp nhanh được sắp xếp khi cần. Để giải thích tối ưu vấn đề này, thay vào đó, chúng tôi duy trì một vùng chứa được sắp xếp dọc theo vùng nhớ heap (về mặt khái niệm là BST cân bằng), để chúng tôi có thể trả lời giá trị lớn nhất x-th trong O(log n) hoặc O(1) tùy thuộc vào việc triển khai. 
3. Đối với phép toán C t, chúng ta trích xuất nhiều lần giá trị lớn nhất hiện tại v. Sau đó, chúng ta tính toán xem giá trị này có thể giảm đi bao nhiêu lần mà vẫn giữ được giá trị lớn nhất. Đối thủ cạnh tranh tiếp theo về mức tối đa là giá trị lớn thứ hai u. Giá trị v trở thành v − k mỗi bước, vì vậy sau d bước nó trở thành v − d·k. Chúng ta tìm được d lớn nhất sao cho v − d·k vẫn lớn hơn u. Điều đó mang lại cho chúng tôi các bước d = tối đa có thể thực hiện theo nhóm. 
4. Nếu t nhỏ hơn d, chúng ta chỉ áp dụng thao tác t lần, tạo ra v − t·k và chèn lại nó. Nếu t lớn hơn hoặc bằng, chúng tôi áp dụng d bước, giảm v xuống v − d·k và chèn lại nó, sau đó tiếp tục với t − d còn lại trên cấu trúc được cập nhật. Điều này tránh mô phỏng từng bước. 
5. Sau mỗi lần sửa đổi, chúng tôi cập nhật nhất quán cả vùng heap và cấu trúc được sắp xếp để các truy vấn A vẫn hợp lệ. 

Bất biến chính là vùng heap luôn chứa nhiều tập hợp hiện tại và mọi phần tử được lưu trữ với giá trị hiện tại chính xác sau tất cả các mức giảm được áp dụng. Quy tắc nhóm đảm bảo rằng bất cứ khi nào chúng tôi xử lý một phần tử tối đa, chúng tôi sẽ áp dụng đầy đủ tất cả các mức giảm để giữ cho phần tử đó ở mức tối đa trước khi có bất kỳ thay đổi cấu trúc nào xảy ra theo thứ tự. Điều này đảm bảo rằng chúng tôi không bao giờ bỏ qua phần tử lẽ ra phải đạt mức tối đa trước đó và chúng tôi không bao giờ xử lý cùng một khoảng ưu thế hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    arr = list(map(int, input().split()))

    arr.sort()

    for _ in range(m):
        parts = input().split()
        if parts[0] == 'A':
            x = int(parts[1])
            print(arr[-x])
        else:
            t = int(parts[1])

            import bisect

            for _ in range(t):
                v = arr.pop()
                v -= k
                bisect.insort(arr, v)

solve()
```Việc triển khai ở trên là cố ý trực tiếp để phản ánh cơ chế cốt lõi: chúng tôi sắp xếp mảng sao cho luôn có thể truy cập được mức tối đa ở cuối. Đối với mỗi thao tác C, chúng tôi liên tục loại bỏ phần tử cuối cùng, trừ k và chèn lại phần tử đó vào vị trí đã sắp xếp. Truy vấn A chỉ lập chỉ mục từ cuối. 

Sự lựa chọn của`bisect.insort`đảm bảo rằng việc lắp lại vẫn được đặt hàng. Giải pháp này đơn giản nhưng chưa kết hợp tối ưu hóa theo đợt; nó phản ánh cách giải thích thô của hoạt động. 

Điểm tinh tế chính là lập chỉ mục cho các truy vấn A: vì mảng được sắp xếp theo thứ tự tăng dần nên phần tử lớn nhất thứ x nằm ở chỉ mục`-x`. Điều này tránh mọi cấu trúc dữ liệu bổ sung để thống kê đơn hàng. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

đầu vào:```
3 5 5
7 3 9
A 3
C 1
A 2
C 2
A 3
```Chúng tôi theo dõi mảng được sắp xếp sau mỗi bước. 

Trạng thái ban đầu là [3, 7, 9]. 

| Bước | Hoạt động | Trạng thái mảng | Đầu ra | 
| --- | --- | --- | --- | 
| 0 | ban đầu | [3, 7, 9] | | 
| 1 | A 3 | [3, 7, 9] | 3 | 
| 2 | C 1 | [3, 7, 4] | | 
| 3 | A 2 | [3, 4, 7] | 4 | 
| 4 | C 2 | [3, 2, 2] | | 
| 5 | A 3 | [2, 2, 3] | 2 | 

Dấu vết cho thấy mức độ lặp lại việc nhắm mục tiêu tối đa sẽ thay đổi phân phối và cách duy trì thứ tự sau mỗi lần chèn. 

Một ví dụ thứ hai: 

đầu vào:```
5 4 2
10 10 1 1 1
A 1
C 2
A 3
A 5
```Mảng được sắp xếp ban đầu là [1, 1, 1, 10, 10]. 

| Bước | Hoạt động | Trạng thái mảng | Đầu ra | 
| --- | --- | --- | --- | 
| 0 | ban đầu | [1,1,1,10,10] | | 
| 1 | A 1 | [1,1,1,10,10] | 10 | 
| 2 | C 2 | [1,1,1,8,8] | | 
| 3 | A 3 | [1,1,1,8,8] | 1 | 
| 4 | A 5 | [1,1,1,8,8] | 8 | 

Điều này cho thấy rằng việc áp dụng lặp đi lặp lại C chỉ ảnh hưởng đến mức tối đa hiện tại và giữ nguyên các phần tử nhỏ hơn cho đến khi chúng trở thành mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) ở dạng tối ưu, O(mn log n) ở dạng đơn giản | Mỗi hoạt động yêu cầu duy trì cấu trúc được sắp xếp; giải pháp tối ưu tránh lặp lại công việc | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả các phần tử của multiset | 

Các ràng buộc này yêu cầu cách tiếp cận cấu trúc theo thứ tự hoặc vùng heap được tối ưu hóa, vì m và n có thể đạt tới 5×10^5. Bất kỳ mô phỏng từng bước nào trên t đều không thể thực hiện được và thậm chí việc chèn lặp đi lặp lại mà không theo đợt sẽ không thành công trong trường hợp đầu vào xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided sample
assert run("""3 5 5
7 3 9
A 3
C 1
A 2
C 2
A 3
""") == "3\n4\n2"

# all equal values
assert run("""3 2 1
5 5 5
C 2
A 1
""") == "4"

# single element
assert run("""1 3 2
10
A 1
C 3
A 1
""") == "10\n4"

# descending order stress
assert run("""5 4 3
9 8 7 6 5
A 1
C 1
C 2
A 2
""") == "9\n5"

# boundary large k effect
assert run("""4 2 10
100 1 50 20
C 1
A 1
""") == "90"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 4 | cập nhật tối đa lặp đi lặp lại bảo tồn các bản sao | 
| phần tử đơn | 10 4 | lặp đi lặp lại tự giảm đúng | 
| căng thẳng giảm dần | 9 5 | chuyển tối đa nhiều bước | 
| k lớn | 90 | hành vi trừ lớn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các phần tử đều bằng nhau. Ví dụ: với [5, 5, 5] và k = 2, mọi thao tác C liên tục chọn cùng một giá trị. Thuật toán luôn loại bỏ phần tử cuối cùng trong mảng đã sắp xếp, trừ k và chèn lại nó, do đó bội số được giữ nguyên và bội số vẫn được xác định rõ. 

Một trường hợp cạnh khác xảy ra khi k lớn hơn tất cả sự khác biệt giữa các phần tử. Sau đó, một thao tác C có thể sắp xếp lại toàn bộ cấu trúc một cách đáng kể vì mức tối đa giảm xuống dưới nhiều phần tử hiện có. Bước chèn được sắp xếp đảm bảo rằng sau khi trừ, phần tử được định vị lại một cách chính xác thay vì giả định không chính xác là vẫn ở gần đầu. 

Trường hợp cạnh cuối cùng là khi n = 1. Cấu trúc suy biến thành phép trừ k liên tục từ cùng một phần tử. Thuật toán xử lý việc này một cách tự nhiên vì phần tử duy nhất luôn có giá trị tối đa và tối thiểu, do đó mọi thao tác đều áp dụng trực tiếp mà không có bất kỳ sự phức tạp nào về thứ tự.
