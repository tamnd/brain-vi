---
title: "CF 104393D - Phá hủy tiểu hành tinh"
description: "Chúng ta được cung cấp một lưới dọc có nhiều hàng và cột, nhưng hầu như tất cả các ô đều trống ngoại trừ một tiểu hành tinh trong mỗi cột. Cột $i$ chứa chính xác một tiểu hành tinh được đặt ở hàng $ci$."
date: "2026-07-01T01:21:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "D"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 77
verified: true
draft: false
---

[CF 104393D - Phá hủy các tiểu hành tinh](https://codeforces.com/problemset/problem/104393/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới dọc có nhiều hàng và cột, nhưng hầu như tất cả các ô đều trống ngoại trừ một tiểu hành tinh trong mỗi cột. Cột$i$chứa chính xác một tiểu hành tinh xếp ở hàng$c_i$. Tàu vũ trụ xuất phát ở vị trí$(0, 0)$và nó có thể di chuyển lên xuống trong cột hiện tại. Hành động quan trọng là nó có thể bắn ra một chùm tia bất cứ lúc nào và chùm tia đó sẽ phá hủy mọi tiểu hành tinh nằm cùng hàng với vị trí hiện tại của tàu vũ trụ. 

Sự chuyển động giữa các cột được ẩn chứa trong tuyên bố: vì các tiểu hành tinh nằm rải rác trên các cột nhưng việc bắn ảnh hưởng đến toàn bộ một hàng, nên cấu trúc thực tế là mỗi tiểu hành tinh tương ứng với một điểm mục tiêu$(i, c_i)$. Con tàu có thể định vị lại theo chiều dọc đến bất kỳ hàng nào mà không tốn kém về mặt hạn chế, nhưng số lần bắn bị giới hạn ở mức$K$và mỗi phát bắn chỉ hữu ích nếu nó được bắn vào một hàng chứa ít nhất một tiểu hành tinh chưa bị phá hủy. 

Vì vậy nhiệm vụ chỉ còn là lựa chọn nhiều nhất$K$chỉ số hàng sao cho tổng số cột có tiểu hành tinh nằm trên các hàng đó là tối đa. 

Mỗi hàng được chọn “bao phủ” tất cả các cột có chiều cao tiểu hành tinh bằng giá trị hàng đó và mỗi cột đóng góp tối đa một lần vì chỉ có một tiểu hành tinh trên mỗi cột. 

Các ràng buộc rất lớn:$C$có thể đi lên$10^5$. Bất kỳ giải pháp nào thử tất cả các tập hợp con của hàng hoặc thậm chí tất cả các kết hợp trình tự chụp đều không khả thi. Thậm chí$O(C \log C)$hoặc$O(C)$mỗi truy vấn đều có thể chấp nhận được, nhưng mọi thứ bậc hai trên hàng hoặc cột sẽ quá chậm. 

Một trường hợp cạnh tinh tế đến từ độ cao lặp đi lặp lại. Nếu nhiều cột có chung$c_i$, một lần bắn vào hàng đó sẽ xóa tất cả chúng. Một cách tiếp cận đơn giản xử lý các cột một cách độc lập thay vì tổng hợp theo hàng sẽ tính toán quá nhiều hành động và đánh giá thấp hiệu quả. 

Một trường hợp cạnh khác là khi$K$vượt quá số lượng độ cao khác nhau. Trong trường hợp đó, nên chọn mọi hàng riêng biệt có ít nhất một tiểu hành tinh, nhưng cách tiếp cận tham lam theo cột ngây thơ vẫn có thể lãng phí số lần chụp. 

Ví dụ, hãy xem xét:```
C = 5, K = 5
c = [1, 1, 2, 2, 3]
```Câu trả lời đúng là 5, vì tất cả các tiểu hành tinh đều bị xóa sạch bằng 3 phát bắn. Một cách tiếp cận đơn giản là kích hoạt trên mỗi cột có thể cho rằng cần nhiều ảnh hơn hoặc xử lý sai các mục trùng lặp. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là nghĩ về trình tự các cảnh quay. Vì tàu vũ trụ có thể tự do di chuyển theo chiều dọc nên mỗi lần bắn sẽ chọn một hàng một cách hiệu quả và tất cả các tiểu hành tinh trên hàng đó sẽ bị loại bỏ. Một giải pháp ngây thơ sẽ thử tối đa tất cả các tập hợp con của các hàng có kích thước$K$, tính xem có bao nhiêu cột được che phủ và lấy kết quả tốt nhất. Điều này đúng, vì thứ tự không quan trọng, chỉ có hàng nào được chọn. 

Tuy nhiên, số lượng tập hợp con có thể có của các hàng là theo cấp số nhân theo số chiều cao khác nhau. Trong trường hợp xấu nhất, tất cả$c_i$khác nhau nên có$C$hàng ứng cử viên. Đang thử tất cả$K$-sự kết hợp dẫn đến$\binom{C}{K}$, điều này là không thể thực hiện được ngay cả đối với nhỏ$K$. Ngay cả một giải pháp quay lui cũng sẽ bùng nổ về mặt tổ hợp. 

Quan sát chính là các hàng độc lập: việc chọn một hàng đóng góp chính xác số lượng cột có tiểu hành tinh nằm ở đó và không có sự tương tác giữa các hàng ngoại trừ ngân sách$K$. Vì vậy, vấn đề trở thành: chúng ta có nhiều tập trọng số (tần số của mỗi chiều cao hàng) và chúng ta muốn chọn nhiều nhất$K$các phần tử có tổng tổng tối đa, trong đó việc chọn một hàng sẽ mang lại tần số của nó. 

Điều này ngay lập tức giảm xuống việc sắp xếp tần số theo thứ tự giảm dần và chiếm vị trí cao nhất$K$. Mỗi lần bắn phải được sử dụng trên một hàng để mang lại mức tăng cận biên tối đa và vì mức tăng là cố định và độc lập nên lựa chọn tham lam là tối ưu. 

Chúng tôi tính toán bản đồ tần số của tất cả$c_i$, sắp xếp tần số và tính tổng lớn nhất$K$các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con hàng |$O(2^C)$|$O(C)$| Quá chậm | 
| Đếm tần số + sắp xếp |$O(C \log C)$|$O(C)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách độ cao của tiểu hành tinh và đếm số lần mỗi độ cao xuất hiện. 

Bước này nhóm tất cả các cột có thể bị phá hủy chỉ bằng một lần bắn, vì việc bắn vào một hàng sẽ loại bỏ tất cả các tiểu hành tinh trên hàng đó. 
2. Trích xuất tất cả tần số thành một mảng. 

Mỗi tần số thể hiện lợi ích của việc chọn hàng đó một lần. 
3. Sắp xếp tần số theo thứ tự giảm dần. 

Chúng tôi muốn ưu tiên các hàng có khả năng loại bỏ tiểu hành tinh nhiều nhất trong mỗi lần chụp. 
4. Lấy cái đầu tiên$K$các giá trị từ danh sách đã sắp xếp và tính tổng chúng. 

Nếu có ít hơn$K$các hàng riêng biệt, chúng tôi lấy tất cả chúng vì những cú đánh thêm không thể cải thiện kết quả. 
5. Xuất ra tổng tính toán. 

### Tại sao nó hoạt động 

Each row contributes a fixed value independent of other rows, because each asteroid belongs to exactly one row and is removed the first time that row is chosen. Điều này tạo ra một tập hợp các phần thưởng độc lập và mỗi lần bắn có thể chọn chính xác một phần thưởng. Since there is no overlap between rewards of different rows, maximizing total destruction reduces to selecting the largest available rewards up to$K$. Bất kỳ sự sai lệch nào so với việc nhận phần thưởng lớn nhất hiện có còn lại chỉ có thể thay thế khoản đóng góp lớn hơn bằng khoản đóng góp nhỏ hơn, điều này không thể cải thiện số tiền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    R, C, K = map(int, input().split())
    c = list(map(int, input().split()))

    freq = {}
    for x in c:
        freq[x] = freq.get(x, 0) + 1

    vals = sorted(freq.values(), reverse=True)

    ans = 0
    for i in range(min(K, len(vals))):
        ans += vals[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén cấu trúc lưới thành bản đồ tần số theo hàng. Đây là sự đơn giản hóa quan trọng giúp loại bỏ mọi sự phụ thuộc vào chuyển động hoặc hình học. Bước sắp xếp đảm bảo chúng tôi luôn xem xét các hàng có lợi nhất trước tiên. 

Vòng lặp được giới hạn cẩn thận bởi cả hai$K$và số lượng hàng riêng biệt, giúp tránh lỗi lập chỉ mục khi$K$là lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 1
2 2 1
```Tần số là: 

- hàng 2 → 2 tiểu hành tinh 
- hàng 1 → 1 tiểu hành tinh 

| Bước | Các hàng có sẵn (tần số) | Được chọn | Còn K | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| bắt đầu | [2, 1] | - | 1 | 0 | 
| 1 | [2, 1] | 2 | 0 | 2 | 

Đầu ra là 2. 

Điều này cho thấy rằng mặc dù có nhiều cột nhưng một lựa chọn hàng tối ưu sẽ tối đa hóa phạm vi bao phủ. 

### Mẫu 2 

đầu vào:```
2 3 3
2 2 1
```Tần số: 

- hàng 2 → 2 
- hàng 1 → 1 

| Bước | Các hàng có sẵn (tần số) | Được chọn | Còn K | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| bắt đầu | [2, 1] | - | 3 | 0 | 
| 1 | [2, 1] | 2 | 2 | 2 | 
| 2 | [1] | 1 | 1 | 3 | 

Đầu ra là 3. 

Điều này xác nhận rằng những lần chụp bổ sung vượt quá số lượng hàng hữu ích sẽ không làm thay đổi kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(C \log C)$| Tần số đếm là tuyến tính, việc sắp xếp các độ cao khác nhau chiếm ưu thế | 
| Không gian |$O(C)$| Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi độ cao riêng biệt | 

Các ràng buộc cho phép lên đến$10^5$giá trị, do đó nghiệm tuyến tính nằm trong giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính theo số chiều cao tiểu hành tinh riêng biệt, vừa vặn trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter

    R, C, K = map(int, input().split())
    c = list(map(int, input().split()))

    freq = Counter(c)
    vals = sorted(freq.values(), reverse=True)

    ans = sum(vals[:K])
    return str(ans)

# provided samples
assert run("3 3 1\n2 2 1\n") == "2", "sample 1"
assert run("2 3 3\n2 2 1\n") == "3", "sample 2"
assert run("3 3 2\n1 2 3\n") == "2", "sample 3"

# custom cases
assert run("1 1 1\n1\n") == "1", "single asteroid"
assert run("5 5 10\n1 1 1 1 1\n") == "5", "all same row"
assert run("5 5 2\n1 2 3 4 5\n") == "2", "all distinct, limited shots"
assert run("5 6 3\n1 1 2 2 3 3\n") == "6", "balanced frequencies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tiểu hành tinh đơn | 1 | tính đúng đắn của trường hợp tối thiểu | 
| tất cả cùng một hàng | 5 | nhiều bức ảnh vs một hàng sụp đổ | 
| tất cả các bức ảnh khác biệt, có giới hạn | 2 | lựa chọn tham lam trên các hàng duy nhất | 
| tần số cân bằng | 6 | nhóm tối ưu các khối bằng nhau | 

## Vỏ cạnh 

Một trường hợp phổ biến là khi tất cả các tiểu hành tinh nằm trên cùng một hàng. Ví dụ:```
3 3 10
5 5 5
```Bản đồ tần số chỉ chứa một mục nhập, 3. Thuật toán sắp xếp [3] và lấy min(K, 1), tạo ra 3. Bất kỳ lý do nào cố gắng “sử dụng nhiều ảnh trên mỗi cột” sẽ không chính xác vì mô hình chỉ cho phép hủy một lần trên mỗi hàng cho mỗi lần chụp. 

Một trường hợp cạnh khác là khi$K = 0$. Danh sách tần số có thể không trống, nhưng thuật toán trả về 0 một cách chính xác vì nó không bao giờ chọn bất kỳ hàng nào khi cắt theo$K$. Một vòng lặp ngây thơ trên các tần số mà không bảo vệ các bức ảnh bằng 0 có thể tích lũy các giá trị không chính xác. 

Trường hợp tinh vi cuối cùng là tính đa dạng lớn:```
C = 5, K = 2
c = [1, 2, 3, 4, 5]
```Thuật toán sắp xếp tất cả một và chọn hai, tạo ra 2. Bất kỳ cách tiếp cận nào cố gắng “di chuyển con tàu” giữa các cột thay vì coi các hàng là phần thưởng độc lập có thể làm phức tạp quá mức chuyển động một cách không chính xác, nhưng chi phí di chuyển không liên quan đến việc tính điểm, vì vậy chỉ có lựa chọn mới quan trọng.
