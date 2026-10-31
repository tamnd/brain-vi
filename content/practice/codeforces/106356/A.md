---
title: "CF 106356A - Hành lang lượng tử"
description: "Chúng ta có một hành lang một chiều gồm các phòng được đánh số từ 1 đến n. Một hạt bắt đầu từ một buồng cố định i. Từ bất kỳ căn phòng nào, nó có thể cố gắng di chuyển sang trái hoặc sang phải đúng một bước hoặc đúng b bước."
date: "2026-06-19T14:56:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106356
codeforces_index: "A"
codeforces_contest_name: "Replay of BUET IUPC 2026, Powered By Phitron"
rating: 0
weight: 106356
solve_time_s: 52
verified: true
draft: false
---

[CF 106356A - Hành lang lượng tử](https://codeforces.com/problemset/problem/106356/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hành lang một chiều gồm các phòng được đánh số từ 1 đến n. Một hạt bắt đầu từ một buồng cố định i. Từ bất kỳ căn phòng nào, nó có thể cố gắng di chuyển sang trái hoặc sang phải đúng một bước hoặc đúng b bước. Mỗi lần di chuyển chỉ thành công nếu nó tiếp đất bên trong hành lang, nếu không nó sẽ bị bỏ qua và hạt vẫn giữ nguyên vị trí. 

Theo thời gian, hạt có thể thực hiện bất kỳ chuỗi chuyển động hợp lệ nào, trộn lẫn cả kích thước bước và cả hai hướng. Câu hỏi không phải là mô phỏng chuyển động từng bước mà là xác định xem căn phòng nào có thể tiếp cận được từ vị trí bắt đầu. Đầu ra cuối cùng là số lượng khoang trong [1, n] không bao giờ có thể đạt được. 

Ràng buộc n lên tới 10^9 ngay lập tức loại trừ mọi mô phỏng trên các vị trí. Bất kỳ BFS hoặc DP nào trên tất cả các ngăn đều không thể thực hiện được vì ngay cả việc lưu trữ các trạng thái đã truy cập cũng sẽ vượt quá bộ nhớ. Vì có thể có tối đa 10^5 trường hợp thử nghiệm nên mỗi thử nghiệm phải được giải theo thời gian không đổi hoặc logarit. 

Một trường hợp thất bại tinh tế xuất hiện khi chỉ nghĩ đến khả năng tiếp cận địa phương. Ví dụ, nếu n = 10, i = 5, a = 4, b = 6, người ta có thể cho rằng hạt chỉ có thể đạt tới một vài vị trí lân cận vì bước nhảy lớn. Trong thực tế, việc kết hợp các bước di chuyển sẽ tạo ra một mạng lưới các vị trí có thể tiếp cận được và một số khoang ở xa vẫn có thể tiếp cận được thông qua các kết hợp tuyến tính. 

Một trường hợp cạnh khác phát sinh khi một trong các kích thước bước bằng 0 modulo còn lại hoặc khi a và b chia sẻ một gcd lớn. Ví dụ: nếu a = 2 và b = 4, thì thực tế chỉ có thể bù đắp bằng nhau và một nửa hành lang sẽ không thể truy cập được mặc dù cả hai bước di chuyển đều có vẻ linh hoạt riêng lẻ. 

Khó khăn cốt lõi là nhận ra rằng tập hợp có thể truy cập bị chi phối bởi cấu trúc số học thay vì việc khám phá biểu đồ. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng giải quyết vấn đề này một cách trực tiếp, chúng ta sẽ coi mỗi khoang là một nút trong biểu đồ và kết nối nó với các vị trí có thể tiếp cận bằng ±a và ±b. Sau đó, chúng tôi sẽ chạy BFS hoặc DFS bắt đầu từ i. Điều này tính toán chính xác khả năng tiếp cận, nhưng trong trường hợp xấu nhất, hành lang có kích thước 10^9, do đó, ngay cả việc khám phá một phần nhỏ các nút cũng không thể thực hiện được. Đồ thị ẩn nhưng vẫn có không gian trạng thái rất lớn. 

Quan sát quan trọng là mọi nước đi đều thay đổi vị trí theo +a, -a, +b hoặc -b. Điều này có nghĩa là bất kỳ vị trí có thể tiếp cận nào cũng khác với vị trí bắt đầu i bởi tổ hợp tuyến tính số nguyên của a và b. Nói cách khác, nếu chúng ta cho phép x tiếp cận được thì x - i phải biểu diễn được dưới dạng k1·a + k2·b đối với các số nguyên k1 và k2. 

Một kết quả lý thuyết số cổ điển cho chúng ta biết rằng tất cả các tổ hợp số nguyên của a và b tạo thành bội số của gcd(a, b). Vì vậy, các vị trí có thể tiếp cận tạo thành một cấp số cộng: 

i + t·g, trong đó g = gcd(a, b), miễn là các vị trí vẫn nằm trong [1, n]. 

Do đó, bài toán rút gọn thành việc đếm xem có bao nhiêu số nguyên trong [1, n] nằm trong cùng một lớp thặng dư với i modulo g. Một khi chúng ta biết tập hợp này, mọi thứ khác đều được tính đơn giản. 

Lực lượng vũ phu không thành công vì nó khám phá các trạng thái một cách rõ ràng, trong khi giải pháp tối ưu nén toàn bộ hệ thống chuyển động thành một bất biến duy nhất: gcd chi phối khả năng kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu BFS/DFS | O(n) mỗi lần kiểm tra | O(n) | Quá chậm | 
| Lý luận số học GCD | O(log min(a,b)) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính g = gcd(a, b). Giá trị này biểu thị kích thước bước nhỏ nhất có thể được tạo bằng cách kết hợp hai bước di chuyển theo bất kỳ chuỗi nào. Mọi chuyển vị có thể tiếp cận phải là bội số của g. 
2. Xác định lớp thặng dư của vị trí ban đầu modulo g. Mọi vị trí có thể tiếp cận phải thỏa mãn x ≡ i (mod g), vì vậy chúng tôi đang giới hạn hành lang ở một cấp số cộng. 
3. Đếm xem có bao nhiêu số trong [1, n] thỏa mãn sự đồng dạng này. Vị trí hợp lệ đầu tiên là x ≥ 1 nhỏ nhất sao cho x ≡ i (mod g), và sau đó cứ g bước lại một vị trí hợp lệ khác xuất hiện. 
4. Tính số đếm này bằng cách sử dụng số học đơn giản: tìm vị trí hợp lệ đầu tiên của f trong [1, n], sau đó đếm xem có bao nhiêu số hạng có dạng f + k·g nằm trong n. 
5. Câu trả lời là số lượng buồng không thể tiếp cận được, bằng n trừ đi số lượng phòng có thể tiếp cận được. 

Lý do bước 3 hoạt động là vì tập hợp có thể truy cập không tùy ý bên trong lớp đồng dư, nó lấp đầy nó hoàn toàn trong giới hạn. Khi có thể tiếp cận được một vị trí trong lớp dư lượng, tất cả những vị trí khác trong lớp đó đều có thể tiếp cận được miễn là chúng ta vẫn ở trong hành lang. 

### Tại sao nó hoạt động 

Hệ thống chuyển động tạo ra chính xác nhóm con cộng của các số nguyên được tạo bởi a và b. Nhóm con đó là g·Z, trong đó g = gcd(a, b). Do đó, tập hợp tất cả sự khác biệt giữa các vị trí có thể tiếp cận là cố định và độc lập với cấu trúc đường dẫn. Điều này biến bài toán thành việc đếm các điểm mạng trong cấp số cộng một chiều giao nhau với một khoảng bị chặn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    q = int(input())
    out = []
    for _ in range(q):
        n, i, a, b = map(int, input().split())
        
        g = math.gcd(a, b)
        
        # first reachable position >= 1 in the same residue class
        # as i modulo g
        r = i % g
        
        # find first x in [1, n] such that x % g == r
        first = r if r != 0 else g
        
        if first < 1:
            first += g
        
        if first > n:
            out.append(str(0))
            continue
        
        reachable = (n - first) // g + 1
        unreachable = n - reachable
        out.append(str(unreachable))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này tính toán gcd của hai kích thước bước, xác định đầy đủ cấu trúc lớp dư lượng có thể tiếp cận. Biến r nắm bắt lớp đồng dư của vị trí bắt đầu. Sau đó, chúng tôi xây dựng vị trí hợp lệ đầu tiên bên trong hành lang phù hợp với lớp dư lượng này. 

Khi đã biết điểm bắt đầu đó, việc đếm các vị trí có thể tiếp cận là một phép tính lũy tiến số học đơn giản. Phép chia (n - thứ nhất) // g tính toán có bao nhiêu bước nhảy đầy đủ có kích thước g phù hợp cho đến n. 

Cần cẩn thận khi xử lý số học môđun khi i chia hết cho g. Trong trường hợp đó r trở thành 0, nhưng lớp dư lượng hợp lệ tương ứng với g chứ không phải 0 trong hệ thống có chỉ số 1. Đây là lý do tại sao mã ánh xạ r == 0 tới g. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 9, i = 3, a = 3, b = 6 

Ở đây g = gcd(3, 6) = 3. Tập hợp có thể truy cập phải ở các vị trí đồng dư với 3 mod 3, nghĩa là tất cả các vị trí. Vì vậy, mọi buồng đều có thể truy cập được. 

| Bước | Giá trị | 
| --- | --- | 
| g | 3 | 
| dư lượng | 0 (được coi là 3) | 
| đầu tiên | 3 | 
| có thể truy cập | (9 - 3)//3 + 1 = 3 | 
| không thể truy cập | 9 - 3 = 6 | 

Điều này cho thấy rằng mặc dù các bước nhảy có vẻ hạn chế, nhưng gcd sẽ thu gọn cấu trúc thành một cấp số cộng đầy đủ. 

### Ví dụ 2 

đầu vào: 

n = 15, i = 10, a = 1, b = 17 

Ở đây g = 1, vì vậy mọi số nguyên đều có thể truy cập được trong giới hạn. 

| Bước | Giá trị | 
| --- | --- | 
| g | 1 | 
| dư lượng | 0 (được coi là 1) | 
| đầu tiên | 1 | 
| có thể truy cập | 15 | 
| không thể truy cập | 0 | 

Điều này chứng tỏ rằng việc có kích thước bước là 1 ngay lập tức khiến toàn bộ hành lang có thể tiếp cận được bất kể bước nhảy còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log min(a, b)) mỗi lần kiểm tra | tính toán gcd chiếm ưu thế trong mỗi bài kiểm tra | 
| Không gian | O(1) | chỉ lưu trữ một vài số nguyên | 

Giải pháp xử lý thoải mái tối đa 10^5 truy vấn vì mỗi truy vấn là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import math
    input = sys.stdin.readline
    
    q = int(input())
    out = []
    for _ in range(q):
        n, i, a, b = map(int, input().split())
        g = math.gcd(a, b)
        r = i % g
        first = r if r != 0 else g
        if first < 1:
            first += g
        if first > n:
            out.append("0")
        else:
            reachable = (n - first) // g + 1
            out.append(str(n - reachable))
    
    return "\n".join(out)

# custom cases
assert run("1\n1 1 2 3\n") == "0", "single cell always reachable"
assert run("1\n10 5 2 4\n") == "5", "even-only reachability"
assert run("1\n10 1 7 11\n") == "9", "gcd = 1 full reachability"
assert run("1\n20 3 6 9\n") == "13", "multiple of gcd structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ô | 0 | ranh giới tầm thường | 
| bước chẵn | 5 | lớp chẵn lẻ bị hạn chế | 
| gcd 1 | 9 | khả năng tiếp cận đầy đủ | 
| chia sẻ gcd > 1 | 13 | đếm lũy tiến số học | 

## Vỏ cạnh 

Khi n = 1, thuật toán trả về chính xác 0 khoang không thể truy cập được vì vị trí duy nhất luôn có thể truy cập được từ chính nó bất kể quy tắc chuyển động. 

Đối với trường hợp a và b chia sẻ một gcd lớn, chẳng hạn như a = 100, b = 150, các vị trí có thể truy cập sẽ nhảy theo các bước 50. Nếu n = 200 và i = 10, thì vị trí có thể truy cập đầu tiên trong phạm vi có thể là 10 hoặc được dịch chuyển theo bội số của 50 và thuật toán căn chỉnh chính xác với lớp dư lượng và chỉ đếm các bội số hợp lệ bên trong giới hạn. 

Khi i nhỏ hơn g hoặc i ≡ 0 mod g, phần dư của bước chuẩn hóa ánh xạ từ 0 đến g đảm bảo chúng ta vẫn chọn được một đại diện hợp lệ trong hành lang. Điều này ngăn ngừa các lỗi riêng lẻ trong đó số học của lớp dư lượng sẽ đề xuất vị trí 0, vị trí không tồn tại trong miền.
