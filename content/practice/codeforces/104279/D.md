---
title: "CF 104279D - \u5c0f\u7f8e\u7231\u753b\u9c7c"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi đang làm việc trên một lưới ở góc phần tư thứ nhất. Mỗi phân đoạn chúng tôi nhận được nằm trên một đường chéo 45 độ, bởi vì điểm cuối của mỗi phân đoạn thỏa mãn cùng một giá trị $x + y$."
date: "2026-07-01T21:11:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104279
codeforces_index: "D"
codeforces_contest_name: "21st UESTC Programming Contest - Preliminary"
rating: 0
weight: 104279
solve_time_s: 65
verified: true
draft: false
---

[CF 104279D - \u5c0f\u7f8e\u7231\u753b\u9c7c](https://codeforces.com/problemset/problem/104279/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, chúng tôi đang làm việc trên một lưới ở góc phần tư thứ nhất. Mỗi phân đoạn chúng tôi nhận được nằm trên một đường chéo 45 độ, bởi vì điểm cuối của mỗi phân đoạn thỏa mãn cùng một giá trị$x + y$. Điều này có nghĩa là mọi đoạn đều bị hạn chế di chuyển từ một điểm$(x_1, y_1)$đến một điểm khác$(x_2, y_2)$đồng thời đi trên cùng một đường chéo, di chuyển nghiêm ngặt sang phải và đi xuống. 

Về mặt hình học, mỗi đoạn như vậy vạch ra một phần của một đường chéo cố định trong lưới. Các đoạn khác nhau có thể nằm trên các đường chéo khác nhau tùy thuộc vào$x + y$giá trị. 

Hai điều cần thiết cho mỗi trường hợp thử nghiệm. Đầu tiên, chúng ta phải xác định xem có ô lưới đơn vị nào có đường chéo đi qua nhiều lần hay không. Vì mỗi ô đơn vị đóng góp chính xác một đoạn đường chéo (cùng hướng dốc), điều kiện này tương đương với việc đảm bảo rằng dọc theo mọi đường chéo cố định, các đoạn được vẽ không trùng nhau theo cách khiến bất kỳ điểm nào trên đường thẳng bị che phủ nhiều lần. 

Thứ hai, chúng ta phải tính tổng chiều dài của tất cả các đoạn được vẽ, nhưng bài toán yêu cầu tổng chiều dài này chia cho 2. Đóng góp của mỗi đoạn chỉ phụ thuộc vào chuyển vị ngang của nó, vì chuyển vị dọc được xác định bởi ràng buộc đường chéo. 

Các ràng buộc cho phép lên đến$10^5$phân đoạn cho mỗi trường hợp thử nghiệm và tối đa 10 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ việc kiểm tra chồng chéo theo cặp bậc hai nào. Thậm chí một$O(n \log^2 n)$cách tiếp cận cho mỗi trường hợp thử nghiệm sẽ là ranh giới nếu được triển khai với các hằng số nặng, vì vậy chúng ta nên hướng tới$O(n \log n)$hoặc tốt hơn cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại khó phát hiện nếu chúng ta bỏ qua việc nhóm theo đường chéo. Ví dụ: hãy xem xét các phân đoạn:$(0,1)\to(1,0)$Và$(1,2)\to(2,1)$. Chúng nằm trên các đường chéo khác nhau, vì vậy chúng không bao giờ tương tác và việc kiểm tra chồng chéo trên chúng sẽ là vô nghĩa. Một cách tiếp cận đơn giản sắp xếp tất cả các phân khúc trên toàn cầu theo$x_1$sẽ so sánh chúng không chính xác và có thể báo cáo sự trùng lặp sai. 

Một trường hợp lỗi khác xuất hiện nếu chúng ta chỉ kiểm tra điểm cuối. Hai đoạn trên cùng một đường chéo có thể không chia sẻ điểm cuối nhưng vẫn chồng chéo một phần bên trong đoạn đó, điều này sẽ vi phạm điều kiện. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ so sánh từng cặp đoạn thẳng nằm trên cùng một đường chéo. Đối với mỗi cặp, chúng tôi sẽ kiểm tra xem các khoảng của chúng trên đường đó có giao nhau với độ dài dương hay không. Điều này đòi hỏi$O(n^2)$so sánh trong trường hợp xấu nhất, quá chậm khi$n = 10^5$. Ngay cả khi bị giới hạn trên mỗi đường chéo, một đường chéo vẫn có thể chứa hầu hết tất cả các đoạn. 

Quan sát quan trọng là mỗi đoạn chỉ đơn giản là một khoảng trên đường một chiều được xác định bởi$c = x + y$. Khi chúng tôi nhóm các phân đoạn theo giá trị này, vấn đề sẽ trở thành vấn đề phát hiện chồng chéo khoảng thời gian cổ điển cho mỗi nhóm. Chúng tôi chỉ cần sắp xếp các khoảng trong mỗi nhóm và xác minh rằng chúng không giao nhau ngoài điểm cuối. 

Nhiệm vụ thứ hai, tính toán một nửa tổng chiều dài được vẽ, đơn giản hóa vì mỗi đoạn đều đóng góp chính xác nhịp ngang của nó.$(x_2 - x_1)$đến câu trả lời cần thiết. Tổng hợp các giá trị này trên tất cả các phân đoạn sẽ mang lại kết quả cuối cùng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra theo cặp Brute Force |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp theo đường chéo và khoảng thời gian kiểm tra |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

Đầu tiên, chúng tôi nhóm các phân đoạn theo giá trị$c = x + y$. Đây là danh tính của đường chéo mà đoạn nằm trên đó. Mỗi phân khúc thuộc về chính xác một nhóm như vậy. 

Thứ hai, trong mỗi nhóm, chúng tôi coi mỗi phân đoạn là khoảng một chiều$[x_1, x_2]$, từ$y$được xác định duy nhất bởi$x + y = c$. 

Sau đó chúng tôi sắp xếp các khoảng này theo điểm cuối bên trái của chúng$x_1$. Việc sắp xếp đảm bảo rằng nếu có bất kỳ sự trùng lặp nào tồn tại thì nó phải xuất hiện giữa các khoảng thời gian liên tiếp theo thứ tự này. 

Tiếp theo, chúng tôi quét qua các khoảng đã sắp xếp và duy trì điểm ngoài cùng bên phải đã đạt được cho đến nay. Nếu chúng tôi gặp một khoảng mới có điểm bắt đầu nhỏ hơn ranh giới bên phải hiện tại, chúng tôi sẽ ngay lập tức phát hiện sự trùng lặp và đánh dấu câu trả lời là không hợp lệ. Nếu điểm bắt đầu bằng ranh giới, điều này được cho phép vì việc chạm vào điểm cuối không bao hàm phạm vi phủ sóng lặp lại. 

Đồng thời, chúng tôi tích lũy sự đóng góp của từng phân đoạn cho câu trả lời cuối cùng bằng cách thêm$x_2 - x_1$. 

Cuối cùng, chúng tôi xuất ra liệu điều kiện chồng chéo có bị vi phạm hay không và tổng được tính toán. 

Tính chính xác đến từ việc giảm mỗi đường chéo thành một đường một chiều. Trên một đường như vậy, mọi vùng phủ sóng lặp lại phải xuất hiện dưới dạng giao điểm theo khoảng thời gian theo thứ tự được sắp xếp. Vì việc sắp xếp duy trì tính liền kề của các khoảng có khả năng chồng chéo nên một lần quét tuyến tính duy nhất là đủ để phát hiện tất cả các vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        groups = defaultdict(list)
        total = 0
        ok = True

        for _ in range(n):
            x1, y1, x2, y2 = map(int, input().split())
            c = x1 + y1
            groups[c].append((x1, x2))
            total += (x2 - x1)

        for c, segs in groups.items():
            segs.sort()
            r = -1
            for l, rr in segs:
                if l < r:
                    ok = False
                    break
                r = max(r, rr)
            if not ok:
                break

        print("YES" if ok else "NO")
        print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng một từ điển được khóa theo định danh đường chéo$x+y$. Mỗi mục được lưu trữ được giảm xuống một khoảng thời gian trên$x$, vì ràng buộc đường chéo làm cho$y$dư thừa. Việc kiểm tra chồng chéo chỉ được thực hiện trong mỗi nhóm sau khi sắp xếp. 

Tổng hoạt động được tích lũy ngay lập tức trong quá trình phân tích cú pháp đầu vào, vì mỗi phân đoạn đóng góp độc lập$x_2 - x_1$. Điều này tránh việc phải xem lại các phân đoạn sau này. 

Một điểm tinh tế là chúng tôi cho phép các khoảng liền kề trong đó$l = r$. Điều này tương ứng với các phân đoạn chạm vào các điểm cuối mà không có phạm vi bao phủ kép của bất kỳ điểm bên trong nào, điều này hợp lệ trong điều kiện có vấn đề. 

## Ví dụ đã hoạt động 

Xét trường hợp có hai đoạn thẳng trên cùng một đường chéo: 

đầu vào:$$(0,1)\to(2,-1)\ \text{is invalid geometrically so instead use valid: } (0,1)\to(2,-1) \text{ignored}$$Một ví dụ đúng: 

Phân đoạn:$(0,1)\to(1,0)$,$(1,1)\to(2,0)$nằm trên các đường chéo khác nhau nên chúng không bao giờ tương tác. 

| Phân đoạn | c = x+y | Khoảng thời gian | 
| --- | --- | --- | 
| (0,1)-(1,0) | 1 | [0,1] | 
| (1,1)-(2,0) | 2 | [1,2] | 

Mỗi nhóm độc lập nên không xảy ra sự chồng chéo và câu trả lời là CÓ. 

Bây giờ hãy xem xét các phân đoạn chồng chéo: 

đầu vào:$(0,1)\to(2,-1)$lại không hợp lệ, vì vậy phiên bản chính xác:$(0,1)\to(2,-1)$được thay thế bằng các ví dụ đường chéo hợp lệ:$(0,2)\to(2,0)$Và$(1,1)\to(3,-1)$lại không hợp lệ; thay vào đó: 

Sử dụng nhất quán c=2:$(0,2)\to(2,0)$

$(1,1)\to(3,-1)$vẫn không hợp lệ do y âm nên điều chỉnh tên miền: 

Ví dụ tốt hơn:$(0,2)\to(2,0)$

$(1,1)\to(2,0)$Thứ hai thực sự là$(1,1)\to(2,0)$, tương tự c=3. 

Bây giờ cả hai đều có đường chéo khác nhau nên vẫn không có sự trùng lặp. 

Để minh họa chính xác sự chồng chéo trên cùng một đường chéo: 

Tất cả các điểm phải thỏa mãn x+y=c và y không âm: 

Lấy c=4:$(0,4)\to(2,2)$

$(1,3)\to(3,1)$| Phân đoạn | Khoảng thời gian | 
| --- | --- | 
| (0,4)-(2,2) | [0,2] | 
| (1,3)-(3,1) | [1,3] | 

Sắp xếp mang lại$[0,2]$,$[1,3]$. Lần thứ hai bắt đầu trước khi lần đầu tiên kết thúc, do đó, sự trùng lặp được phát hiện và câu trả lời trở thành KHÔNG. 

Điều này xác nhận rằng quá trình quét xác định chính xác giao lộ bên trong chứ không chỉ xung đột ở điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi phân đoạn được chèn vào một nhóm và sắp xếp theo đường chéo, tổng sắp xếp trên tất cả các nhóm chiếm ưu thế | 
| Không gian |$O(n)$| Tất cả các phân đoạn được lưu trữ một lần, được nhóm theo đường chéo | 

Các ràng buộc cho phép lên đến$10^5$phân đoạn cho mỗi trường hợp thử nghiệm, do đó$O(n \log n)$giải pháp phù hợp thoải mái trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            n = int(input())
            g = defaultdict(list)
            total = 0
            ok = True
            for _ in range(n):
                x1,y1,x2,y2 = map(int,input().split())
                c = x1+y1
                g[c].append((x1,x2))
                total += x2-x1

            for segs in g.values():
                segs.sort()
                r = -1
                for l,rr in segs:
                    if l < r:
                        ok = False
                        break
                    r = max(r, rr)
                if not ok:
                    break

            out.append(("YES" if ok else "NO") + "\n" + str(total))
        return "\n".join(out)

    return solve()

# simple non-overlap
assert run("1\n2\n0 1 1 0\n1 2 2 1\n") == "YES\n2", "no overlap"

# overlap on same diagonal
assert run("1\n2\n0 4 2 2\n1 3 3 1\n") == "NO\n4", "overlap case"

# single segment
assert run("1\n1\n0 0 5 5\n") == "YES\n5", "single segment"

# disjoint multiple diagonals
assert run("1\n3\n0 1 1 0\n1 2 2 1\n2 3 3 2\n") == "YES\n3", "separate diagonals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | CÓ | trường hợp cơ sở đúng đắn | 
| khoảng chồng chéo | KHÔNG | phát hiện chồng chéo trên cùng một đường chéo | 
| nhiều đường chéo | CÓ | sự độc lập của nhóm | 

## Vỏ cạnh 

Một trường hợp khó khăn là khi các đoạn chỉ chạm vào điểm cuối. Ví dụ,$[0,2]$Và$[2,5]$trên cùng một đường chéo không vi phạm điều kiện vì không có điểm đơn vị nào bị che hai lần. Thuật toán xử lý việc này vì nó cho phép sự bình đẳng$l = r$mà không gây ra lỗi và chỉ gắn cờ chồng chéo nghiêm ngặt khi$l < r$. 

Một trường hợp khác là khi tất cả các đoạn nằm trên các đường chéo khác nhau. Ngay cả khi các phép chiếu của chúng trùng nhau theo thứ tự x toàn cầu, chúng vẫn độc lập. Việc phân nhóm theo$x+y$đảm bảo chúng không bao giờ can thiệp và quá trình quét không bao giờ so sánh các khoảng thời gian không liên quan. 

Trường hợp tinh tế cuối cùng là đầu vào lớn trong đó tất cả các phân đoạn có cùng đường chéo. Trong trường hợp đó, thuật toán suy biến thành một lần quét theo khoảng thời gian được sắp xếp duy nhất, vẫn chạy trong$O(n \log n)$do phân loại và vẫn nằm trong giới hạn.
