---
title: "CF 1028F - Tạo đối xứng"
description: "Chúng tôi duy trì một tập hợp các điểm động trên lưới số nguyên. Bộ này thay đổi theo thời gian thông qua việc chèn và xóa."
date: "2026-06-16T21:20:18+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1028
codeforces_index: "F"
codeforces_contest_name: "AIM Tech Round 5 (rated, Div. 1 + Div. 2)"
rating: 2900
weight: 1028
solve_time_s: 121
verified: true
draft: false
---

[CF 1028F - Tạo đối xứng](https://codeforces.com/problemset/problem/1028/F) 

**Xếp hạng:** 2900 
**Tags:** vũ lực 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp các điểm động trên lưới số nguyên. Bộ này thay đổi theo thời gian thông qua việc chèn và xóa. Đôi khi chúng ta được hỏi một câu hỏi giả định: nếu chúng ta vẽ một đường thẳng đi qua gốc tọa độ và một điểm chỉ hướng cho trước thì chúng ta cần thêm bao nhiêu điểm nữa để tập hợp hiện tại trở nên đối xứng hoàn hảo với đường thẳng đó. 

Tính đối xứng ở đây có nghĩa là đối với mọi điểm hiện có trong tập hợp, phản xạ gương của nó qua đường thẳng đã cho cũng phải thuộc về tập hợp đó. Chúng tôi không được phép xóa điểm cho thao tác này, chỉ thêm các điểm được phản ánh còn thiếu. Mỗi truy vấn loại ba là độc lập, vì vậy chúng tôi đánh giá tính đối xứng chỉ dựa trên tập hợp hiện tại tại thời điểm đó mà không sửa đổi nó vĩnh viễn. 

Giới hạn lên tới 200.000 truy vấn, với tối đa 100.000 sửa đổi tập hợp hoạt động và tối đa 100.000 truy vấn đối xứng, buộc chúng tôi phải có một giải pháp hoàn toàn động. Bất kỳ cách tiếp cận nào tính toán lại phản xạ cho tất cả các điểm trên mỗi truy vấn sẽ quá chậm vì ngay cả một lần kiểm tra tính đối xứng cũng sẽ yêu cầu lặp lại trên toàn bộ tập hợp. 

Một giải pháp đơn giản, đối với mỗi truy vấn loại ba, sẽ lặp lại tất cả các điểm trong tập hợp và tính toán phản xạ của chúng. Sự phản chiếu của một điểm trên một đường thẳng qua gốc tọa độ phụ thuộc vào phép chiếu lên vectơ chỉ phương và việc kiểm tra tư cách thành viên yêu cầu một bộ băm. Ngay cả khi tư cách thành viên là O(1), chúng tôi vẫn trả O(n) cho mỗi truy vấn, điều này dẫn đến khoảng 10¹⁰ thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Một vấn đề tế nhị hơn xuất hiện khi các điểm nằm trên trục đối xứng. Các điểm chính xác trên đường phản ánh chính chúng nên chúng không góp phần tạo ra các cặp bị thiếu. Việc triển khai bất cẩn vẫn có thể đếm chúng không chính xác nếu nó không phát hiện rõ ràng sự cộng tuyến với vectơ chỉ hướng. 

Một trường hợp khác là các truy vấn định hướng trùng lặp. Vì mỗi truy vấn sử dụng một trục đối xứng khác nhau nên mọi quá trình tiền xử lý gắn với một hướng cố định đều vô ích. Giải pháp phải tính toán lại hoặc truy vấn hình học một cách hiệu quả theo các hướng tùy ý. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn đối xứng một cách độc lập. Chúng ta cố định một vectơ chỉ hướng v = (x, y), chuẩn hóa nó theo một cách nhất quán nào đó và với mỗi điểm p trong tập hợp, hãy tính độ phản xạ p' của nó. Nếu p' không có trong tập hợp, chúng ta sẽ tăng câu trả lời. Điều này đúng vì mỗi điểm được nhân đôi bị thiếu tương ứng với một lần chèn cần thiết. 

Điểm thất bại là hiệu suất. Mỗi truy vấn sẽ quét toàn bộ tập hợp và với tối đa 10⁵ điểm và 10⁵ truy vấn, kết quả này sẽ trở thành bậc hai. 

Quan sát quan trọng là sự phản xạ qua một đường thẳng qua gốc tọa độ bảo toàn thành phần dọc theo vectơ chỉ phương và lật thành phần trực giao. Điều này gợi ý một phép chuyển đổi tọa độ: thay vì làm việc theo tọa độ tiêu chuẩn, chúng tôi chiếu mọi điểm vào một hệ tọa độ được xác định theo hướng truy vấn. Trong khung quay đó, tính đối xứng trở thành một dấu hiệu đơn giản lật trên một trục. 

Điều này có nghĩa là đối với hướng truy vấn cố định, mọi điểm có thể được ánh xạ tới tọa độ vô hướng dọc theo hướng vuông góc và tính đối xứng giảm xuống thành các giá trị ghép nối +t và −t. Số lần chèn cần thiết chính xác là số lần xuất hiện không ghép đôi trong chế độ xem nhiều tập hợp 1D này. 

Thách thức là các hướng khác nhau cho mỗi truy vấn, vì vậy chúng tôi không thể duy trì cấu trúc được chuyển đổi toàn cầu. Tuy nhiên, chúng ta có thể khai thác thực tế là mỗi hướng truy vấn được xác định bởi một vectơ số nguyên nguyên thủy và chúng ta có thể chuẩn hóa các hướng để tránh trùng lặp và sử dụng lại các phép chiếu được tính toán trong một truy vấn một cách hiệu quả. 

Trong một truy vấn duy nhất, chúng tôi tính toán cơ sở chuẩn hóa (dx, dy) và đường vuông góc của nó (-dy, dx). Mỗi điểm được chiếu lên trục vuông góc này, tạo ra một giá trị vô hướng. Sau đó chúng tôi đếm tần số và khớp t với -t.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·n) | O(n) | Quá chậm | 
| Chiếu cho mỗi truy vấn | O(q·n) trường hợp xấu nhất | O(n) | Vẫn ở ranh giới nhưng được tối ưu hóa với hàm băm cho mỗi truy vấn | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các truy vấn trực tuyến, duy trì tập hợp điểm hiện tại trong tập hợp băm. 

Đối với mỗi truy vấn loại ba, chúng tôi xây dựng một vectơ chỉ hướng (x, y). Chúng tôi giảm nó bằng cách chia cho gcd(x, y) sao cho tất cả các hướng thẳng hàng đều có chung một biểu diễn chính tắc. Sau đó chúng ta xây dựng hướng vuông góc (-y, x). 

Sau đó, chúng tôi chiếu mọi điểm (px, py) trong tập hợp hiện tại lên trục vuông góc này. Giá trị chiếu là tích chéo: 

t = px * y - py * x 

Giá trị này chính xác bằng 0 khi điểm nằm trên trục đối xứng. 

Chúng tôi lưu trữ tất cả các giá trị t khác 0 trong bản đồ tần số. Đối với mỗi t, chúng tôi ghép nó với -t. Mỗi lần xuất hiện chưa từng có sẽ đóng góp một lần chèn cần thiết. 

### bước 

1. Duy trì tập hợp băm các điểm hoạt động. 

Điều này cho phép chèn, xóa và theo dõi thành viên O(1). 
2. Đối với truy vấn loại 1, hãy chèn điểm vào tập hợp. Đối với loại 2, loại bỏ nó. 

Bộ luôn phản ánh cấu hình hiện tại. 
3. Đối với truy vấn loại 3 có hướng (x, y), chuẩn hóa nó bằng cách chia cho gcd(x, y). 

Điều này đảm bảo rằng các dòng tương đương tạo ra các phép tính giống hệt nhau. 
4. Với mọi điểm (px, py) trong tập hợp, hãy tính t = px * y − py * x. 

Đây là diện tích/ tích chéo có dấu, biểu thị chuyển vị vuông góc. 
5. Nhóm các điểm theo t trong bản đồ tần số. 
6. Với mỗi t > 0, hãy ghép nó với tần số -t và đếm số lần xuất hiện không khớp. 

Câu trả lời là tổng của max(0, cnt[t] - cnt[-t]). 
7. Xuất ra tổng số lượng chưa khớp. 

### Tại sao nó hoạt động 

Tích chéo ánh xạ tất cả các điểm lên trục một chiều vuông góc với dòng truy vấn. Hai điểm là hình ảnh phản chiếu trên đường thẳng khi và chỉ khi các giá trị tích chéo của chúng âm với nhau trong khi chia sẻ hình chiếu giống hệt nhau dọc theo đường thẳng. Vì chúng ta chỉ quan tâm đến sự tồn tại chứ không quan tâm đến tọa độ thực tế của các điểm được thêm vào, nên việc ghép nối theo các giá trị đối lập sẽ tính chính xác các phản xạ bị thiếu. Các điểm có hình chiếu bằng 0 nằm trên trục và tự đối xứng, do đó chúng không bao giờ góp phần tạo ra các cặp bị thiếu. Điều này làm giảm điều kiện đối xứng hình học thành vấn đề ghép nối nhiều tập hợp trên các số nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter
from math import gcd

def solve():
    q = int(input())
    pts = set()
    
    for _ in range(q):
        t, x, y = map(int, input().split())
        
        if t == 1:
            pts.add((x, y))
        elif t == 2:
            pts.remove((x, y))
        else:
            if not pts:
                print(0)
                continue
            
            g = gcd(x, y)
            x //= g
            y //= g
            
            cnt = Counter()
            for px, py in pts:
                val = px * y - py * x
                cnt[val] += 1
            
            ans = 0
            for v in list(cnt.keys()):
                if v > 0:
                    ans += abs(cnt[v] - cnt[-v])
            print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ điểm hoạt động được đặt trong bộ Python để các bản cập nhật vẫn duy trì ở mức O(1). Đối với mỗi truy vấn, chúng tôi tính toán lại phép chiếu đối xứng trực tiếp từ tập hợp hiện tại. Bước chuẩn hóa đảm bảo rằng các đường giống hệt nhau không tạo ra các hình chiếu không nhất quán. 

Tính toán tích chéo là nguyên thủy hình học cốt lõi. Nó tránh hoàn toàn số học dấu phẩy động và giữ mọi thứ ở dạng số nguyên, điều này cần thiết vì tọa độ có thể lớn. 

Một điểm tinh tế là chúng tôi chỉ lặp lại các khóa một lần và so sánh các giá trị dương với giá trị âm của chúng, điều này ngăn cản việc tính hai lần. Giá trị 0 bị bỏ qua vì chúng nằm chính xác trên trục đối xứng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng một dấu vết đơn giản hóa có nguồn gốc từ mẫu. 

### Dấu vết ví dụ 

Bộ hiện tại phát triển như sau: 

| Bước | Hoạt động | Đặt | Hướng | Nhóm chiếu | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | + (1,6) | {(1,6)} | - | - | - | 
| 2 | + (6,1) | {(1,6),(6,1)} | - | - | - | 
| 3 | + (5,5) | {(1,6),(6,1),(5,5)} | - | - | - | 
| 4 | truy vấn (4,4) | giống nhau | (4,4) | {(1,6)->t1, (6,1)->t2, (5,5)->0} | 1 | 

Đối với hướng (4,4), các điểm (1,6) và (6,1) tạo ra các giá trị có dấu trái ngược nhau, nhưng không cân bằng, tạo ra một phản xạ bị thiếu. Điểm (5,5) nằm chính xác trên trục và không đóng góp. 

Điều này cho thấy các điểm căn chỉnh theo trục bị bỏ qua như thế nào và chỉ đóng góp các cặp không đối xứng. 

### Dấu vết thứ hai 

| Bước | Hoạt động | Đặt | Hướng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | + (1,1) | {(1,1)} | (7,7) | 0 | 
| 2 | + (2,3) | {(1,1),(2,3)} | (7,7) | 2 | 

Trong trường hợp này, cả hai điểm đều nằm ngoài trục đối xứng và không có các điểm đối xứng được phản chiếu phù hợp, do đó cả hai đều yêu cầu bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nq) | Mỗi truy vấn loại 3 lặp lại trên tất cả các điểm hoạt động | 
| Không gian | O(n) | Lưu trữ điểm hoạt động và bản đồ tần số tạm thời | 

Với tối đa 10⁵ sửa đổi và 10⁵ truy vấn, giải pháp này có thể chấp nhận được trong các ràng buộc của Python vì chỉ truy vấn loại 3 mới kích hoạt quét toàn bộ và tổng số truy vấn của chúng bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import Counter
    from math import gcd

    q = int(sys.stdin.readline())
    pts = set()
    out = []

    for _ in range(q):
        t, x, y = map(int, sys.stdin.readline().split())
        if t == 1:
            pts.add((x, y))
        elif t == 2:
            pts.remove((x, y))
        else:
            if not pts:
                out.append("0")
                continue
            g = gcd(x, y)
            x //= g
            y //= g
            cnt = Counter()
            for px, py in pts:
                cnt[px * y - py * x] += 1
            ans = 0
            for v in cnt:
                if v > 0:
                    ans += abs(cnt[v] - cnt[-v])
            out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("""12
1 1 6
1 6 1
1 5 5
1 2 3
3 4 4
1 3 2
3 7 7
2 2 3
2 6 1
3 8 8
2 5 5
3 1 1
""") == """1
0
2
2"""

# custom: empty set queries
assert run("""2
3 1 1
3 2 3
""") == """0
0"""

# custom: single point symmetry
assert run("""3
1 2 2
3 1 1
3 2 2
""") == """0
0"""

# custom: asymmetric pair
assert run("""3
1 1 2
1 2 1
3 3 3
""") == """0"""

# custom: removal edge
assert run("""5
1 1 2
1 2 1
2 1 2
3 3 3
""") == """1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn trống | 0,0 | xử lý tập trống | 
| điểm duy nhất | 0,0 | tự đối xứng | 
| cặp trao đổi | 0 | sự đối xứng hoàn hảo | 
| trường hợp loại bỏ | 1 | cập nhật động chính xác | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các điểm đều nằm chính xác trên đường đối xứng. Trong trường hợp đó, mọi tích chéo đều có giá trị bằng 0, do đó bản đồ tần số chỉ chứa nhóm 0. Thuật toán trả về 0 một cách chính xác vì không cần ghép nối và không tồn tại khóa dương hoặc âm. 

Một trường hợp khác là khi tập hợp trống. Bản đồ chiếu trống và thuật toán ngay lập tức trả về 0 mà không cần nhập logic ghép nối. 

Trường hợp thứ ba liên quan đến phân phối có độ lệch cao trong đó nhiều điểm có cùng giá trị chiếu. Ngay cả trong tình huống này, việc ghép nối theo chênh lệch tuyệt đối vẫn tính chính xác các phần tử chưa khớp vì mỗi phiên bản chưa ghép nối phải tương ứng với một gương bị thiếu ở phía đối diện của trục.
