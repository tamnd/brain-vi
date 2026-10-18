---
title: "CF 105582G - Kính giải pháp"
description: "Chúng tôi được cung cấp một số thùng chứa, mỗi thùng chứa dung dịch lỏng. Đối với mỗi thùng chứa, chúng ta biết hai giá trị: tổng khối lượng của dung dịch và bao nhiêu khối lượng trong đó là muối."
date: "2026-06-22T17:51:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105582
codeforces_index: "G"
codeforces_contest_name: "Ural Championship 2017"
rating: 0
weight: 105582
solve_time_s: 65
verified: true
draft: false
---

[CF 105582G - Kính giải pháp](https://codeforces.com/problemset/problem/105582/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số thùng chứa, mỗi thùng chứa dung dịch lỏng. Đối với mỗi thùng chứa, chúng ta biết hai giá trị: tổng khối lượng của dung dịch và bao nhiêu khối lượng trong đó là muối. Chúng tôi muốn chọn một tập hợp con không trống trong số các thùng chứa này, đổ mọi thứ lại với nhau và kiểm tra xem hỗn hợp thu được có nồng độ muối rất cụ thể hay không. 

Đối với bất kỳ tập hợp con nào được chọn, nồng độ cuối cùng được xác định bằng tỷ lệ tổng khối lượng muối trên tổng khối lượng. Nhiệm vụ là đếm xem có bao nhiêu tập hợp con tạo ra chính xác phân số a/b cần thiết sau khi trộn. 

Khó khăn chính là điều kiện này không mang tính cộng tính một cách đơn giản. Chúng tôi không chỉ tính tổng các giá trị và so sánh với một ngưỡng, chúng tôi phải thực thi sự bình đẳng hợp lý chính xác giữa các tập hợp con. 

Ràng buộc n 35 là gợi ý cấu trúc chính. Có thể liệt kê trực tiếp tất cả các tập hợp con trong O(2^n), nhưng 2^35 là quá lớn. Điều này ngay lập tức gợi ý việc chia mảng và kết hợp các kết quả từng phần. 

Trường hợp cạnh tinh tế xuất phát từ những đóng góp trống ở một phía của phần phân chia. Ví dụ: nếu tập hợp con hợp lệ duy nhất được hình thành hoàn toàn ở nửa bên phải hoặc hoàn toàn ở nửa bên trái hoặc một phần từ cả hai, chúng tôi phải đảm bảo rằng chúng tôi không vô tình đếm tập hợp con trống vì vấn đề yêu cầu tổng thể lựa chọn không trống. 

Một trường hợp góc khác xuất hiện khi a = 0. Khi đó phần mục tiêu bằng 0, nghĩa là hỗn hợp cuối cùng phải chứa muối bằng 0. Điều này buộc mọi tổ hợp được chọn phải có tổng muối bằng 0, mặc dù tổng khối lượng có thể dương. Bất kỳ so sánh dấu phẩy động đơn giản nào cũng sẽ thất bại ở đây vì tất cả các phép tính phải duy trì tính chính xác và dựa trên số nguyên. 

## Phương pháp tiếp cận 

Một giải pháp brute-force kiểm tra mọi tập hợp con của n kính, tính toán tổng lượng muối và tổng khối lượng, đồng thời xác minh xem b * Total_salt có bằng a * Total_mass hay không. Điều này đúng và đơn giản về mặt khái niệm. Tuy nhiên, nó kiểm tra 2^n tập hợp con, trong trường hợp xấu nhất là khoảng 3 × 10^10 phép tính khi n = 35, vượt xa mọi giới hạn thực tế. 

Cấu trúc của điều kiện là tuyến tính trong tổng các phần tử được chọn. Nếu chúng ta mở rộng một tập hợp con thành hai phần, trái và phải, thì ràng buộc sẽ trở thành một phương trình tuyến tính duy nhất trên các phần đóng góp kết hợp. Điều này làm cho việc gặp mặt ở giữa có thể áp dụng được. 

Chúng tôi chia mảng thành hai nửa có kích thước tối đa khoảng 17 hoặc 18. Chúng tôi liệt kê tất cả các tập hợp con trong mỗi nửa, tính toán cho từng tập hợp con của cặp (sum_mass, sum_salt). Thay vì cố gắng khớp các cặp trực tiếp theo hai chiều, chúng ta chuyển đổi điều kiện: 

b * (mL + mR) = a * (tL + tR) 

Viết lại mang lại: 

(b * mL - a * tL) + (b * mR - a * tR) = 0 

Điều này có nghĩa là mỗi tập hợp con có thể được gán một giá trị vô hướng duy nhất f = b * m - a * t và các tập hợp con đầy đủ hợp lệ tương ứng chính xác với các cặp tập hợp con trái và phải có tổng giá trị f bằng 0. 

Vì vậy, chúng tôi lưu trữ tần số của các giá trị f cho nửa bên phải và khớp chúng với các giá trị âm ở nửa bên trái. 

Chúng tôi cũng phải loại bỏ kết hợp trống-trống vì đây không phải là tập hợp con không trống hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| Gặp nhau ở giữa | O(2^(n/2)) | O(2^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng bằng cách chia nó thành hai nửa.

1. Chia n kính thành hai nhóm cỡ n1 và n2. Điều này làm giảm không gian tìm kiếm theo cấp số nhân thành hai phần có thể quản lý được. Mỗi nửa sẽ được liệt kê độc lập. 
2. Với mỗi tập con của nửa bên trái, hãy tính tổng khối lượng và tổng số muối, sau đó tính giá trị fL = b * saltL - a * khối lượngL. Lưu trữ tất cả các giá trị như vậy trong một danh sách. Điều này chuyển đổi một ràng buộc hai chiều thành một biểu diễn vô hướng duy nhất. 
3. Lặp lại quá trình tương tự cho nửa bên phải, tạo ra các giá trị fR cho mỗi tập con. Thay vì chỉ lưu trữ danh sách, hãy xây dựng bản đồ tần số từ fR đến số lần xuất hiện của nó. Điều này cho phép tra cứu nhanh khi khớp các giá trị bổ sung. 
4. Điều kiện tập hợp con đầy đủ yêu cầu fL + fR = 0. Với mỗi giá trị trong danh sách bên trái, chúng ta cộng số lượng tập hợp con bên phải có giá trị -fL. Điều này tổng hợp tất cả các kết hợp hợp lệ mà không xây dựng các tập hợp con đầy đủ một cách rõ ràng. 
5. Trừ một nếu cần thiết để loại trừ trường hợp cả hai tập con được chọn đều trống, vì bài toán yêu cầu tổng thể lựa chọn không trống. Tập con trống tương ứng với f = 0 ở cả hai nửa. 

Tính đúng đắn dựa trên thực tế là mỗi tập con của tập hợp đầy đủ tương ứng duy nhất với một cặp tập con của hai nửa và phép biến đổi bảo toàn chính xác điều kiện đẳng thức ban đầu. 

### Tại sao nó hoạt động 

Biểu thức b * sum(salt) - a * sum(mass) là tuyến tính trên các tập hợp con rời rạc. Điều này có nghĩa là giá trị của một tập hợp con kết hợp chính xác là tổng các giá trị từ phần bên trái và bên phải của nó. Do đó, điều kiện tỷ lệ ban đầu tương đương với việc yêu cầu giá trị biến đổi kết hợp bằng 0. Bởi vì mỗi phân rã tập hợp con là duy nhất trong phép chia, việc đếm các phần bù phù hợp sẽ tính mỗi tập hợp con hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def solve():
    n, a, b = map(int, input().split())
    arr = [tuple(map(int, input().split())) for _ in range(n)]
    
    left = arr[:n//2]
    right = arr[n//2:]
    
    def gen(sub):
        res = []
        m = len(sub)
        for mask in range(1 << m):
            sm = 0
            st = 0
            for i in range(m):
                if mask & (1 << i):
                    st += sub[i][0]
                    sm += sub[i][1]
            res.append(b * st - a * sm)
        return res
    
    L = gen(left)
    R = gen(right)
    
    freqR = defaultdict(int)
    for x in R:
        freqR[x] += 1
    
    ans = 0
    for x in L:
        ans += freqR.get(-x, 0)
    
    # exclude empty-empty subset
    # empty subset has value 0 in both halves
    ans -= 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là chia tách mảng để cho phép liệt kê tất cả các tập hợp con trong mỗi nửa. Hàm trợ giúp tạo ra giá trị được chuyển đổi f = b * salt - a * mass cho mọi tập hợp con. Điều này tránh việc lưu trữ các cặp thô và đơn giản hóa việc so khớp. 

Bản đồ tần số được xây dựng cho nửa bên phải sao cho tra cứu phần bù là O(1). Vòng cuối cùng tích lũy các trận đấu từ nửa bên trái. Phép trừ ở cuối sẽ loại bỏ trường hợp không hợp lệ trong đó cả hai nửa chọn các tập con trống cùng một lúc. 

Phải cẩn thận để tất cả số học vẫn ở dạng số nguyên. Việc sử dụng phép chia dấu phẩy động sẽ gây ra lỗi chính xác và phá vỡ việc kiểm tra tính bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 2
1 2
1 2
1 2
1 2
1 4
```Ta chia làm hai nửa: bên trái có 2 phần tử, bên phải có 3 phần tử. 

Đối với mỗi tập hợp con, chúng tôi tính f = 2_salt - 1_mass. 

Tập hợp con bên trái: 

| tập hợp con | muối | khối lượng | f | 
| --- | --- | --- | --- | 
| ∅ | 0 | 0 | 0 | 
| {1} | 1 | 2 | 0 | 
| {2} | 1 | 2 | 0 | 
| {1,2} | 2 | 4 | 0 | 

Các tập hợp con bên phải tạo ra các giá trị cũng thường xuyên có giá trị bằng 0 vì tất cả các tỷ lệ đều khớp với 1/2. 

Việc khớp trái và phải yêu cầu fL + fR = 0, điều này luôn được thỏa mãn ở đây, vì vậy mọi kết hợp đều hoạt động ngoại trừ trống-trống. 

Điều này thể hiện trường hợp suy biến trong đó tất cả các phần tử đã thỏa mãn tỷ lệ mục tiêu riêng lẻ, do đó mọi tập hợp con đều hợp lệ. 

### Ví dụ 2 

đầu vào:```
2 0 1
0 1
1 1
```Chúng ta cần tổng lượng muối bằng 0 vì a = 0. 

Phân chia trái/phải cho các phần tử đơn lẻ. 

Đối với mỗi tập hợp con, f = 1 * muối - 0 * khối lượng = muối. 

| tập hợp con | muối | f | 
| --- | --- | --- | 
| ∅ | 0 | 0 | 
| {0,1} | 0 | 0 | 
| {1,1} | 1 | 1 | 

Chỉ các tập hợp con có tổng lượng muối bằng 0 là hợp lệ. Thuật toán chỉ tính các kết hợp trong đó cả hai nửa đều đóng góp lượng muối bằng 0. 

Điều này xác nhận việc xử lý trường hợp biên a = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n/2)) | Mỗi nửa liệt kê tất cả các tập hợp con và chúng tôi kết hợp thông qua tra cứu hàm băm | 
| Không gian | O(2^(n/2)) | Bản đồ tần số lưu trữ các giá trị tập hợp con được chuyển đổi | 

Việc phân chia đảm bảo tối đa khoảng 2^17 tập hợp con mỗi nửa, tức là khoảng 130.000 trạng thái, dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn bị giới hạn tương tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, a, b = map(int, input().split())
    arr = [tuple(map(int, input().split())) for _ in range(n)]
    
    left = arr[:n//2]
    right = arr[n//2:]
    
    def gen(sub):
        res = []
        m = len(sub)
        for mask in range(1 << m):
            sm = 0
            st = 0
            for i in range(m):
                if mask & (1 << i):
                    st += sub[i][0]
                    sm += sub[i][1]
            res.append(b * st - a * sm)
        return res
    
    L = gen(left)
    R = gen(right)
    
    freqR = defaultdict(int)
    for x in R:
        freqR[x] += 1
    
    ans = 0
    for x in L:
        ans += freqR.get(-x, 0)
    
    ans -= 1
    return str(ans)

# provided sample 1
assert run("""5 1 2
1 2
1 2
1 2
1 2
1 4
""") == "15"

# provided sample 2
assert run("""2 0 1
0 1
1 1
""") == "1"

# custom: single valid only
assert run("""1 1 2
1 2
""") == "1"

# custom: impossible
assert run("""3 1 2
1 1
2 1
3 1
""") == "0"

# custom: all zeros
assert run("""3 0 1
0 1
0 1
0 1
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các tỷ lệ hợp lệ | nhiều tập hợp con | kết hợp tổ hợp đầy đủ | 
| trường hợp a = 0 | tập hợp con bị hạn chế | hạn chế không muối | 
| phần tử đơn | 1 | độ đúng cơ sở | 
| trường hợp bất khả thi | 0 | không có trận đấu ngẫu nhiên | 
| muối hoàn toàn bằng không | tất cả các tập hợp con | bình đẳng thoái hóa | 

## Vỏ cạnh 

Khi a = 0, mọi tập hợp con phải thỏa mãn rằng tổng lượng muối bằng 0. Trong thuật toán, giá trị này trở thành f = b * muối, vì vậy chỉ các tập hợp con có tổng muối bằng 0 mới đóng góp f = 0. Logic khớp vẫn hoạt động vì nó giảm việc đếm các cặp tập hợp con có giá trị bằng 0, giúp lọc chính xác các kết hợp không hợp lệ. 

Khi tất cả các kính riêng lẻ đã thỏa mãn tỷ lệ mục tiêu, mọi tập hợp con sẽ tạo ra f = 0. Thuật toán đếm tất cả các cặp trên một nửa, sau đó trừ đi trường hợp trống rỗng. Điều này phù hợp với thực tế là mọi tập hợp con không trống đều hợp lệ. 

Khi không có tập con nào thỏa mãn phương trình, cả L và R đều chứa giá trị nhưng không tồn tại cặp bổ sung nào, do đó việc tra cứu tần số luôn trả về 0 và câu trả lời vẫn đúng.
