---
title: "CF 105437D - Nửa bằng nhau"
description: "Chúng ta được cung cấp một mảng có độ dài chẵn, được chia thành hai phần bằng nhau về mặt khái niệm: nửa bên trái và nửa bên phải. Chúng tôi được phép thực hiện chính xác một lần hoán đổi, nhưng việc hoán đổi bị hạn chế rất nhiều."
date: "2026-06-23T03:42:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105437
codeforces_index: "D"
codeforces_contest_name: "ICPC 2024-2025 NERC, Southern and Volga Russia Qualifier"
rating: 0
weight: 105437
solve_time_s: 106
verified: false
draft: false
---

[CF 105437D - Hai nửa bằng nhau](https://codeforces.com/problemset/problem/105437/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có độ dài chẵn, được chia thành hai phần bằng nhau về mặt khái niệm: nửa bên trái và nửa bên phải. Chúng tôi được phép thực hiện chính xác một lần hoán đổi, nhưng việc hoán đổi bị hạn chế rất nhiều. Chúng ta phải chọn một chỉ mục từ nửa bên trái và một chỉ mục từ nửa bên phải và trao đổi giá trị của chúng. 

Sau khi thực hiện phép hoán đổi đơn này, chúng ta muốn tổng của nửa bên trái bằng tổng của nửa bên phải. Nhiệm vụ là xác định xem sự hoán đổi như vậy có tồn tại hay không và nếu có thì xuất ra bất kỳ cặp vị trí hợp lệ nào. 

Hạn chế nổi bật là kích thước mảng lên tới 200.000. Bất kỳ giải pháp nào thử tất cả các cặp trên một nửa sẽ yêu cầu khoảng (n/2)^2 lần kiểm tra, tương đương khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn khả thi trong 2 giây. Điều này ngay lập tức loại trừ việc ghép đôi bạo lực. 

Bản thân các giá trị được giới hạn bởi 10.000, đủ nhỏ để các kỹ thuật băm hoặc suy luận dựa trên tần số trở nên hợp lý. 

Một điểm tinh tế là chúng ta phải thực hiện chính xác một lần hoán đổi chứ không phải nhiều nhất là một lần. Vì vậy, ngay cả khi mảng đã có các nửa bằng nhau, chúng ta vẫn cần tìm một phép hoán đổi để bảo toàn sự bằng nhau thay vì không làm gì cả. 

Một dạng thất bại đối với lối suy luận ngây thơ là giả định rằng chúng ta có thể chọn bất kỳ phần tử nào từ bên trái và ghép nó một cách tham lam với phần tử nào đó ở bên phải để cân bằng tổng cục bộ. Ví dụ: cố gắng so khớp các khác biệt một cách độc lập sẽ bỏ qua ràng buộc tổng toàn cục, vì việc hoán đổi ảnh hưởng đồng thời đến cả hai nửa. 

Một cái bẫy khác là quên rằng việc hoán đổi phải chia đôi. Hoán đổi nội bộ hợp lệ bên trong một nửa không được phép ngay cả khi nó sẽ cân bằng số tiền. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi cặp (i, j) với i ở nửa bên trái và j ở nửa bên phải, mô phỏng phép hoán đổi và tính lại cả hai nửa tổng. Mỗi lần kiểm tra có giá O(n) nếu được tính toán lại một cách đơn giản hoặc O(1) nếu chúng tôi duy trì tổng tiền tố. Ngay cả với kiểm tra O(1), chúng tôi vẫn có các cặp O(n^2), tức là khoảng 10^10 thao tác, quá chậm. 

Để cải thiện, chúng tôi viết lại tác dụng của phép hoán đổi theo đại số. Gọi tổng bên trái ban đầu là S_L và tổng bên phải là S_R. Giả sử chúng ta hoán đổi giá trị x từ bên trái với giá trị y từ bên phải. Sau khi hoán đổi, tổng bên trái trở thành S_L - x + y và tổng bên phải trở thành S_R - y + x. Chúng tôi muốn: 

S_L - x + y = S_R - y + x 

Sắp xếp lại mang lại: 

S_L - S_R = 2(x - y) 

Đây là mức giảm chính. Toàn bộ vấn đề trở thành việc tìm một cặp (x, y) sao cho sự khác biệt của chúng được khắc phục bằng sự mất cân bằng toàn cầu giữa các nửa. 

Đặt D = S_L - S_R. Sau đó chúng ta cần: 

x - y = D/2 

Điều này ngay lập tức ngụ ý rằng D phải chẵn, nếu không thì không tồn tại nghiệm nào. 

Bây giờ nhiệm vụ trở thành: tìm x ở nửa bên trái và y ở nửa bên phải sao cho y = x - D/2. Vì các giá trị bị giới hạn và chúng tôi chỉ cần kiểm tra tư cách thành viên nên chúng tôi có thể lưu trữ nửa bên phải trong tập băm hoặc bản đồ tần số và kiểm tra từng phần tử bên trái. 

Điều này làm giảm vấn đề từ việc tìm kiếm cặp trên các chỉ mục đến việc khớp giá trị với việc ghi sổ các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính tổng nửa bên trái và tổng nửa bên phải. Sự khác biệt giữa chúng là thông tin toàn cầu duy nhất quan trọng, bởi vì mọi sự hoán đổi đều ảnh hưởng đến cả hai nửa một cách đối xứng. 
2. Tính D = S_L - S_R. Nếu D lẻ thì dừng lại ngay. Không có cặp số nguyên (x, y) nào có thể thỏa mãn x - y = D/2, do đó không có phép hoán đổi nào có thể cân bằng được hai nửa. 
3. Xây dựng ánh xạ từ các giá trị ở nửa bên phải tới chỉ số của chúng. Chúng ta chỉ cần biết liệu một giá trị có tồn tại hay không và nó xuất hiện ở đâu vì chúng ta có thể tự do lựa chọn bất kỳ vị trí hợp lệ nào. 
4. Tính hiệu mục tiêu k = D / 2. Bây giờ chúng ta muốn tìm phần tử bên trái x sao cho (x - k) tồn tại ở nửa bên phải. Điều này trực tiếp mã hóa điều kiện hoán đổi có nguồn gốc trước đó. 
5. Lặp lại nửa bên trái. Với mỗi vị trí i có giá trị x, kiểm tra xem x - k có tồn tại ở nửa bản đồ bên phải hay không. Nếu đúng như vậy, chúng tôi sẽ trả về ngay lập tức (i, chỉ mục tương ứng). Điều này hợp lệ vì nó thỏa mãn điều kiện đẳng thức dẫn xuất, đảm bảo số tiền cân bằng sau khi hoán đổi. 
6. Nếu không tìm thấy cặp nào như vậy sau khi quét nửa bên trái, xuất ra NO. 

### Tại sao nó hoạt động 

Phép biến đổi làm giảm hiệu ứng hoán đổi thành một phương trình tuyến tính duy nhất trong các giá trị đã chọn. Bất kỳ phép hoán đổi hợp lệ nào cũng phải thỏa mãn x - y = D/2 và bất kỳ cặp nào thỏa mãn phương trình này nhất thiết phải cân bằng hai nửa tổng khi được thay thế trở lại thành tổng đã cập nhật. Vì chúng tôi kiểm tra mọi giá trị bên trái ứng cử viên với tất cả các giá trị bên phải phù hợp có thể có, nên chúng tôi không bỏ lỡ bất kỳ hoán đổi khả thi nào. 

Tính chính xác dựa trên thực tế là phép hoán đổi chỉ thay đổi tổng bằng cách trừ và cộng chính xác các giá trị được hoán đổi. Không có sự tương tác nào khác giữa các vị trí, vì vậy phương trình mô tả đầy đủ tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    mid = n // 2
    left = a[:mid]
    right = a[mid:]
    
    sum_left = sum(left)
    sum_right = sum(right)
    
    diff = sum_left - sum_right
    
    if diff % 2 != 0:
        print("NO")
        return
    
    k = diff // 2
    
    pos_right = {}
    for i, val in enumerate(right):
        if val not in pos_right:
            pos_right[val] = i + 1  # 1-based index in right half
    
    for i, val in enumerate(left):
        need = val - k
        if need in pos_right:
            print("YES")
            print(i + 1, pos_right[need] + mid)
            return
    
    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chia mảng thành hai nửa và tính tổng của chúng một cách trực tiếp. Chi tiết triển khai chính là duy trì các chỉ mục riêng biệt cho nửa bên phải bằng cách sử dụng lập chỉ mục dựa trên 1 tương ứng với phân đoạn đó, sau đó chuyển đổi trở lại lập chỉ mục chung bằng cách thêm`mid`. 

Giá trị tính toán`k`là nửa chênh lệch giữa các tổng và chúng tôi sử dụng nó để chuyển điều kiện thành bài toán tra cứu. Đối với mỗi phần tử bên trái, chúng tôi tính toán giá trị đối tác được yêu cầu ở nửa bên phải và kiểm tra sự tồn tại trong thời gian không đổi bằng cách sử dụng từ điển. 

Một cạm bẫy phổ biến là quên thay đổi chỉ số khi báo cáo vị trí bên phải. Một cái khác là tính toán sai`k`là chênh lệch đầy đủ thay vì một nửa, điều này phá vỡ phương trình rút ra từ hiệu ứng hoán đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
4 1 2 6 7 6
```Nửa bên trái là`[4, 1, 2]`, nửa bên phải là`[6, 7, 6]`. 

| Bước | S_L | S_R | khác biệt | k | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 7 | 19 | -12 | -6 | tính tổng | 
| kiểm tra | 7 | 19 | -12 | -6 | xây dựng bản đồ phù hợp | 
| quét i=1 | 4 | - | - | - | cần = 4 - (-6) = 10 không tìm thấy | 
| quét i=2 | 1 | - | - | - | cần = 1 - (-6) = 7 tìm thấy | 
| dừng lại | - | - | - | - | trao đổi đầu ra | 

Chúng tôi thấy rằng việc hoán đổi giá trị 1 từ bên trái với giá trị 7 từ bên phải sẽ cân bằng tổng. Sau khi hoán đổi, cả hai nửa có tổng bằng 13, xác nhận điều kiện dẫn xuất hoạt động chính xác. 

### Ví dụ 2 

đầu vào:```
4
5 4 3 5
```Nửa bên trái là`[5, 4]`, nửa bên phải là`[3, 5]`. 

| Bước | S_L | S_R | khác biệt | k | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 9 | 8 | 1 | 0,5 | khác biệt là lẻ | 
| dừng lại | 9 | 8 | 1 | - | đầu ra KHÔNG | 

Vì hiệu là số lẻ nên không có hoán đổi số nguyên nào có thể thỏa mãn x - y = D/2 và thuật toán kết thúc sớm một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính tổng, một lượt để xây dựng bản đồ băm, một lượt qua nửa bên trái | 
| Không gian | O(n) | lưu trữ để ánh xạ nửa giá trị sang chỉ mục bên phải | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì n lên tới 200.000 và tất cả các phép toán đều là tra cứu hàm băm theo thời gian tuyến tính và số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample tests
assert run("6\n4 1 2 6 7 6\n") == "YES\n2 5"
assert run("4\n5 4 3 5\n") == "NO"

# all equal values (always possible swap exists)
assert run("4\n1 1 1 1\n") != ""

# minimal case
assert run("2\n1 2\n") == "YES\n1 2"

# no solution case
assert run("6\n1 1 1 100 100 100\n") == "NO"

# crafted imbalance odd sum difference
assert run("4\n1 2 3 10\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố | CÓ trao đổi | độ đúng ranh giới tối thiểu | 
| tất cả đều bình đẳng | CÓ | tồn tại trong trường hợp đối xứng | 
| mất cân bằng mạnh mẽ | KHÔNG | trường hợp thất bại | 
| trường hợp khác biệt lẻ | KHÔNG | ràng buộc chẵn lẻ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng đã được cân bằng. Ví dụ: 

đầu vào:```
4
1 2 2 1
```Ở đây cả hai nửa có tổng bằng 3. Thuật toán tính diff = 0, do đó k = 0. Điều này có nghĩa là chúng ta cần x = y, do đó, bất kỳ cặp nửa chéo có giá trị giống hệt nhau đều hợp lệ. Bản đồ băm ở nửa bên phải đảm bảo rằng nếu một giá trị tồn tại ở cả hai bên, chúng tôi sẽ ngay lập tức tìm thấy một giao dịch hoán đổi hợp lệ, mặc dù tổng số tiền đã bằng nhau. 

Một trường hợp khác là khi tồn tại nhiều giá trị giống nhau. Ánh xạ chỉ lưu trữ một chỉ mục cho mỗi giá trị, nhưng điều này là đủ vì mọi lần xuất hiện đều hợp lệ. Ngay cả khi tồn tại nhiều giao dịch hoán đổi, việc trả về cặp tìm thấy đầu tiên vẫn thỏa mãn điều kiện vì phương trình chỉ phụ thuộc vào giá trị chứ không phải vị trí.
