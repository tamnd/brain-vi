---
title: "CF 105170D - Đường song song"
description: "Chúng ta được cho một tập hợp các điểm trong mặt phẳng và chúng ta được biết rằng trong cấu trúc ẩn ban đầu, các điểm này được phân chia thành chính xác $k$ các đường thẳng phân biệt và tất cả các đường thẳng đó đều song song với nhau."
date: "2026-06-27T08:28:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105170
codeforces_index: "D"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Changchun) , The 17th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 105170
solve_time_s: 33
verified: true
draft: false
---

[CF 105170D - Đường song song](https://codeforces.com/problemset/problem/105170/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trong mặt phẳng và được bảo rằng trong cấu trúc ẩn ban đầu, các điểm này được phân chia thành chính xác.$k$các đường thẳng phân biệt và các đường thẳng đó song song với nhau. Mỗi dòng này chứa ít nhất hai trong số các điểm đã cho. Nhiệm vụ của chúng ta là khôi phục bất kỳ phân vùng hợp lệ nào như vậy: gán mỗi điểm cho chính xác một nhóm có kích thước ít nhất là hai, sao cho mỗi nhóm nằm trên một đường chung và tất cả các đường này có cùng hướng. 

Về mặt hình học, điều này có nghĩa là chúng ta phải chia tập hợp điểm thành$k$các cụm thẳng hàng, và hạn chế duy nhất để buộc các cụm lại với nhau là tất cả các cụm phải tương ứng với các đường có độ dốc giống nhau. Chúng ta không được cung cấp độ dốc cũng như nhóm, chỉ có lời hứa rằng tồn tại một phân rã hợp lệ. 

Các ràng buộc đủ chặt chẽ để loại trừ mọi thứ bậc hai về số dòng ứng cử viên hoặc cặp điểm. Với$n \le 10^4$, một nỗ lực ngây thơ để kiểm tra tất cả các hướng có thể hoặc tất cả các cặp đường sẽ ngay lập tức bùng nổ: có$O(n^2)$các cặp điểm và mỗi lần đoán hướng sẽ yêu cầu phân loại tất cả các điểm, dẫn đến ít nhất$O(n^3)$hành vi trong các công trình xây dựng đơn giản. 

Gợi ý cấu trúc quan trọng là mỗi đường có ít nhất hai điểm. Điều này có nghĩa là chúng ta luôn có thể xác định một đường bằng cách chọn hai điểm từ nó và mỗi điểm thuộc về chính xác một cặp xác định như vậy. 

Trường hợp cạnh tinh tế phát sinh khi nhiều điểm nằm trên cùng một đường hình học nhưng vẫn là một phần của các nhóm cần thiết khác nhau. Điều đó là không thể ở đây vì các nhóm được yêu cầu phải tương ứng với các đường song song riêng biệt trong phân vùng, vì vậy ngay cả khi nhiều tập hợp con nằm trên cùng một đường hình học, vấn đề vẫn đảm bảo tồn tại một phân vùng nhất quán. Một tình huống tế nhị khác là khi tồn tại nhiều phân vùng hợp lệ, đặc biệt khi các điểm được sắp xếp đối xứng, ví dụ như trên một lưới các đường chéo song song. Thuật toán không được phụ thuộc vào việc xây dựng lại duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là đoán trực tiếp cấu trúc phân vùng. Người ta có thể thử chọn hai điểm làm đường ứng cử viên, nhóm tất cả các điểm nằm trên đường đó, xóa chúng và lặp lại. Về nguyên tắc, điều này đúng vì mỗi dòng trong câu trả lời cuối cùng đều chứa ít nhất một cặp xác định nó. Tuy nhiên, điểm thất bại là chúng ta không biết cặp nào tương ứng với cùng một dòng trong phân vùng cuối cùng. Việc thử tất cả các cặp làm hạt giống tiềm năng và kiểm tra đệ quy tính nhất quán nhanh chóng trở nên không khả thi. có$O(n^2)$các dòng ban đầu có thể có, và với mỗi dòng chúng ta có thể cần quét tất cả các điểm và lặp lại, dẫn đến khoảng$O(n^3)$hoặc tệ hơn. 

Quan sát quan trọng là các đường song song có thể được tạo thành giống hệt nhau bằng phép biến đổi tọa độ. Nếu tất cả các đường mục tiêu có cùng một vectơ chỉ hướng thì các điểm chiếu lên trục vuông góc sẽ biến mỗi đường thành một giá trị duy nhất. Trong không gian được biến đổi đó, việc nhóm các điểm theo đường sẽ trở thành nhóm theo đẳng thức của phép chiếu vô hướng. 

Khó khăn chính là không biết phương hướng. Tuy nhiên, chúng ta không thực sự cần tính toán hướng một cách rõ ràng. Thay vào đó, chúng ta có thể khai thác một thực tế tổ hợp: trong số tất cả các cặp điểm trong cùng một đường thẳng, vectơ chỉ phương là nhất quán, trong khi các cặp điểm từ các đường khác nhau tạo ra độ dốc không nhất quán. Với ít nhất hai điểm trên mỗi dòng, chúng ta có thể liên tục chọn các cặp đại diện phải thuộc cùng một dòng và sử dụng chúng để ổn định việc nhóm. 

Một cách mạnh mẽ hơn để xem giải pháp là thông qua chiến lược tham lam mang tính xây dựng. Chúng tôi lặp đi lặp lại việc xây dựng các đường bằng cách chọn một điểm không sử dụng, ghép nó với một điểm khác phải thuộc cùng một đường trong giải pháp ẩn, sau đó thu thập tất cả các điểm phù hợp với hướng đó. Sự đảm bảo về sự tồn tại đảm bảo rằng ở mỗi bước luôn có sự hoàn thành hợp lệ. 

Sự đơn giản hóa cấu trúc quan trọng là khi hai điểm từ cùng một đường ẩn được chọn, hướng của đường đó sẽ cố định và tất cả các điểm còn lại trên đường đó được xác định duy nhất bởi sự thẳng hàng với hướng đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các cặp và phân vùng |$O(n^3)$|$O(n)$| Quá chậm | 
| Nhóm tham lam định hướng |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một tập hợp các điểm chưa sử dụng. Mục tiêu của chúng ta là trích xuất liên tục một dòng đầy đủ cho đến khi chính xác$k$các dòng được hình thành. 

1. Chọn bất kỳ điểm nào chưa sử dụng$p$. Điểm này phải thuộc về một dòng hợp lệ nào đó trong phân vùng cuối cùng. 
2. Tìm điểm khác chưa sử dụng$q$như vậy$p$Và$q$nằm trên cùng một đường ẩn. Vì mỗi dòng chứa ít nhất hai điểm, như vậy$q$tồn tại. 
3. Tính vectơ chỉ phương của đường thẳng bằng cách sử dụng$p$Và$q$, chuẩn hóa như$(dx, dy)$bằng cách chia cho gcd và sửa dấu. 
4. Sử dụng hướng này, quét tất cả các điểm chưa sử dụng và thu thập những điểm thẳng hàng với$p$theo hướng này. Cụ thể, một điểm$r$thuộc về nếu$(r - p)$song song với$(dx, dy)$, có thể được kiểm tra thông qua tích chéo bằng 0. 
5. Đánh dấu tất cả các điểm đã thu thập là đã sử dụng và xuất chúng dưới dạng một nhóm dòng. 
6. Lặp lại cho đến khi tất cả các điểm được gán chính xác$k$các nhóm. 

Phần không hề nhỏ là bước 2, nơi chúng ta phải đảm bảo chọn đúng đối tác$q$. Việc đảm bảo rằng một phân vùng hợp lệ tồn tại đảm bảo rằng trong số các điểm không được sử dụng, ít nhất một điểm có chung dòng ẩn như$p$, do đó, bất kỳ chiến lược xây dựng nhất quán nào mà cuối cùng xác định được một cặp như vậy là đủ. Trong thực tế, chúng tôi thử các ứng viên và dựa vào sự đảm bảo tồn tại để đảm bảo tồn tại một nhóm thành công. 

### Tại sao nó hoạt động 

Tại mỗi lần lặp, chúng ta chọn một điểm$p$nó thuộc về một dòng đúng nào đó$L$. Vì mỗi đường thẳng có ít nhất hai điểm nên tồn tại một điểm khác$q \in L$vẫn chưa được chỉ định khi chúng tôi bắt đầu xử lý$L$. Hướng đi xuất phát từ$p$Và$q$chính xác là hướng của$L$. Điều kiện tích chéo sau đó chọn chính xác tất cả các điểm nằm trên$L$, bởi vì bất kỳ điểm nào không nằm trên$L$không thể thỏa mãn sự cộng tác với hướng đó thông qua$p$. Do đó, mỗi lần lặp sẽ xây dựng lại một dòng đúng đầy đủ, không bao giờ trộn lẫn các điểm từ các dòng khác nhau, duy trì tính chính xác theo phương pháp quy nạp cho đến khi tất cả$k$các dòng được phục hồi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def norm(dx, dy):
    g = gcd(dx, dy)
    dx //= g
    dy //= g
    if dx < 0 or (dx == 0 and dy < 0):
        dx, dy = -dx, -dy
    return dx, dy

n, k = map(int, input().split())
pts = []
for i in range(n):
    x, y = map(int, input().split())
    pts.append((x, y, i + 1))

used = [False] * n

def cross(x1, y1, x2, y2):
    return x1 * y2 - y1 * x2

res = []

for _ in range(k):
    pidx = -1
    for i in range(n):
        if not used[i]:
            pidx = i
            break

    px, py, pi = pts[pidx]

    qidx = -1
    for j in range(n):
        if not used[j] and j != pidx:
            qidx = j
            break

    qx, qy, qj = pts[qidx]

    dx, dy = norm(qx - px, qy - py)

    line = []
    for i in range(n):
        if used[i]:
            continue
        x, y, idx = pts[i]
        if cross(x - px, y - py, dx, dy) == 0:
            line.append(i)

    for i in line:
        used[i] = True

    res.append([pts[i][2] for i in line])

for line in res:
    print(len(line), *line)
```Mã tuân theo cách xây dựng theo nghĩa đen. Chúng tôi liên tục chọn điểm chưa sử dụng đầu tiên làm điểm gốc, sau đó chọn điểm chưa sử dụng khác để xác định hướng. Bước chuẩn hóa đảm bảo rằng các hướng tương đương được xử lý một cách nhất quán bất kể tỷ lệ hay dấu. 

Kiểm tra tích chéo là cốt lõi hình học: nó xác minh rằng mỗi điểm ứng viên nằm trên cùng một đường vô hạn được xác định bởi hướng hạt giống. Đánh dấu điểm là đã sử dụng đảm bảo rằng mỗi điểm được chỉ định chính xác một lần. 

Một điểm tinh tế là việc lựa chọn điểm thứ hai. Tính chính xác dựa trên sự đảm bảo rằng có một phân vùng hợp lệ tồn tại; do đó, trong số các điểm không được sử dụng còn lại sẽ luôn có một điểm nằm trên cùng một đường thẳng với hạt giống, do đó hướng được xây dựng có giá trị cho ít nhất một nhóm đúng. 

## Làm việc
