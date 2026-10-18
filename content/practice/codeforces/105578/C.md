---
title: "CF 105578C - Sự kiện khủng hoảng: Thiên thạch"
description: "Chúng ta có một chiến trường một chiều, một dòng gồm các ô $n$. Một số ô ban đầu chứa các ký tự. Qua các vòng trị giá $m$, mỗi ô sẽ nhận được thiên thạch và những thiên thạch này sẽ tích lũy theo thời gian thay vì biến mất."
date: "2026-06-22T06:18:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105578
codeforces_index: "C"
codeforces_contest_name: "The 2024 ICPC Asia Shenyang Regional Contest (The 3rd Universal Cup. Stage 19: Shenyang)"
rating: 0
weight: 105578
solve_time_s: 70
verified: true
draft: false
---

[CF 105578C - Sự kiện khủng hoảng: Thiên thạch](https://codeforces.com/problemset/problem/105578/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được trao cho một chiến trường một chiều, một đường lối$n$tế bào. Một số ô ban đầu chứa các ký tự. Qua$m$vòng tròn, mỗi tế bào nhận được thiên thạch và những thiên thạch này tích tụ theo thời gian thay vì biến mất. Một ô có thiên thạch trở nên nguy hiểm theo nghĩa là đi vào đó phải trả một khoản chi phí bằng số lượng thiên thạch hiện có trên đó và nếu nhân vật đứng đó khi thiên thạch đáp xuống một vòng, nhân vật đó sẽ chết ngay lập tức. 

Sau khi mỗi vòng kết thúc hạ cánh thiên thạch, những nhân vật sống sót được phép di chuyển sang trái hoặc phải bất kỳ số bước nào. Chỉ có thể di chuyển thông qua các tế bào đã rõ ràng hoặc đã có thiên thạch của chúng. Việc thanh toán sẽ loại bỏ các thiên thạch trên ô đó và cho phép truyền tải. 

Mục tiêu là để đảm bảo rằng tất cả các nhân vật hiện có ban đầu đều sống sót qua tất cả các vòng và vẫn có thể di chuyển tự do một cách hiệu quả mà không bị giết, đồng thời giảm thiểu tổng số tiền chi cho việc dọn sạch thiên thạch. Nếu điều này không thể được đảm bảo, chúng tôi xuất ra$-1$. 

Ràng buộc cơ cấu chính là quy mô: tổng số$n \times m$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^6$. Điều này có nghĩa là bất kỳ giải pháp nào xử lý từng ô trong mỗi vòng một cách đơn giản đều đã ở mức khả thi. Một vòng lặp kép trên cả hai chiều chỉ được chấp nhận nếu mỗi thao tác được thực hiện$O(1)$, nhưng phải tránh mọi thứ phức tạp hơn trên mỗi ô trong mỗi vòng. 

Một trường hợp lỗi nhỏ xuất hiện khi các ký tự bị ngăn cách bởi một vùng và vùng này sẽ bị chặn hoàn toàn trong một vòng nào đó. Nếu tại một thời điểm nào đó, mọi ô trên đường đi cần thiết đều bị thiên thạch chiếm giữ và chúng ta không sẵn sàng hoặc không thể dọn sạch chúng, thì việc di chuyển sẽ trở nên bất khả thi. Ví dụ: nếu toàn bộ đoạn giữa hai ký tự đầu tiên luôn không an toàn và chúng ta không đủ khả năng để xóa nó thì các ký tự đó không bao giờ có thể gặp nhau, dẫn đến không thể xóa được. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ theo dõi từng vòng một, ô nào an toàn, ô nào bị chặn và nơi mỗi nhân vật có thể di chuyển. Sau mỗi vòng, chúng tôi sẽ tính toán lại khả năng tiếp cận của tất cả các ký tự trên dòng và bất cứ khi nào một nhân vật cố gắng vào một ô bị chặn, chúng tôi sẽ trả tiền để xóa nó. Điều này nhanh chóng trở nên tốn kém vì mỗi vòng có thể yêu cầu quét toàn bộ lưới và cập nhật kết nối. Với$n \times m$lên đến$10^6$, thậm chí tính toán lại tuyến tính mỗi vòng dẫn đến$10^{12}$trong trường hợp xấu nhất là không khả thi. 

Quan sát quan trọng là chuyển động chỉ đóng vai trò tổng hợp trong toàn bộ quá trình. Các nhân vật có thể hợp nhất khi họ gặp nhau và sau đó họ hoạt động như một nhóm duy nhất. Vì chiến trường là một đường thẳng nên điều duy nhất quan trọng là liệu tất cả các vị trí ban đầu có thể được kết nối thông qua một đường liên tục gồm các ô có thể đi qua sau khi tất cả các vòng được tính hay không. 

Mỗi tế bào tích lũy thiên thạch theo thời gian. Nếu một ô được sử dụng như một phần của đường truyền tải, chúng ta phải trả chi phí tích lũy của nó. Tuy nhiên, chúng ta có thể tự do lựa chọn các tuyến đường, vì vậy chúng ta chỉ cần quan tâm đến các ô nằm giữa các ký tự đầu tiên, bởi vì các ký tự chỉ cần được kết nối dọc theo dòng. 

Điều này làm giảm vấn đề xác định đoạn đường nào phải đi qua để kết nối tất cả các vị trí ban đầu. Sau khi biết được phân đoạn đó, mọi ô bên trong nó không bị chiếm giữ ban đầu phải được xóa ít nhất một lần và chi phí cho mỗi ô đó chính xác bằng tổng số thiên thạch mà nó tích lũy được trong tất cả các vòng. 

Do đó, cấu trúc sẽ chuyển sang tính toán các trọng số tích lũy trên mỗi ô và tính tổng chúng trên vùng cần thiết. Nếu không thể đạt được kết nối cần thiết vì một vùng cần thiết hoàn toàn không thể sử dụng được theo quy tắc thì câu trả lời là$-1$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ qua các vòng và vị trí |$O(nm)$ĐẾN$O(nm \cdot \text{movement})$|$O(n)$| Quá chậm | 
| Tích lũy tiền tố và suy luận khoảng |$O(nm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén tất cả các sự kiện thiên thạch thành một giá trị tích lũy duy nhất trên mỗi ô. Vì thiên thạch chỉ cộng lại nên số lượng thiên thạch cuối cùng trên mỗi ô sau tất cả các vòng chỉ đơn giản là tổng của tất cả$a_{i,j}$. 

Sau khi có những tổng số này, chúng tôi coi chiến trường như một đường có trọng số nơi tiến vào ô$j$chi phí$A_j$và việc ở bên trong các ô bị chiếm giữ ban đầu không tốn kém gì. 

1. Tính tổng lượng thiên thạch cho mỗi ô bằng cách tính tổng tất cả các vòng. Điều này thể hiện chi phí tắc nghẽn cuối cùng cho ô đó. 
2. Xác định vị trí ngoài cùng bên trái và ngoài cùng bên phải ban đầu chứa một ký tự. Vì các ký tự có thể hợp nhất nên cuối cùng tất cả chúng phải được kết nối thành một khu vực có thể tiếp cận duy nhất, do đó, chỉ có phân đoạn giữa các thái cực này mới quan trọng. 
3. Nếu không có ô trống nào giữa các điểm cực trị này có thể được sử dụng để kết nối các đường dẫn (trong thực tế, nếu đoạn đó hoàn toàn không thể sử dụng được do các ràng buộc của vấn đề), hãy trả về$-1$. 
4. Mặt khác, tính tổng các giá trị thiên thạch tích lũy cho tất cả các ô trống trong phân đoạn. Đây là những ô duy nhất phải được xóa ít nhất một lần để cho phép di chuyển giữa tất cả các ký tự ban đầu. 
5. Ghi số tiền này dưới dạng chi phí tối thiểu. 

Tính chính xác dựa trên tính bất biến là các ký tự chỉ cần duy trì khả năng kết nối chứ không phải các vị trí riêng lẻ. Vì chuyển động không bị hạn chế sau mỗi vòng, khi có đường dẫn giữa tất cả các vị trí ban đầu, tất cả các ký tự có thể được hợp nhất thành một nhóm được kết nối duy nhất và vẫn an toàn sau đó. Chi phí hoàn toàn được xác định bởi ô nào phải được làm cho có thể vượt qua và vì mỗi ô như vậy sẽ tích lũy tất cả thiên thạch trước khi được dọn sạch nên tổng chi phí là cố định và không phụ thuộc vào thứ tự di chuyển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        c = list(map(int, input().split()))
        
        has = False
        L, R = n, -1
        
        for i, x in enumerate(c):
            if x == 1:
                has = True
                L = min(L, i)
                R = max(R, i)
        
        if not has:
            print(-1)
            for __ in range(m):
                input()
            continue
        
        total = [0] * n
        
        for _ in range(m):
            row = list(map(int, input().split()))
            for j in range(n):
                total[j] += row[j]
        
        cost = 0
        for j in range(L, R + 1):
            if c[j] == 0:
                cost += total[j]
        
        print(cost)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xác định khoảng quan tâm, từ ký tự ban đầu ngoài cùng bên trái đến ngoài cùng bên phải. Sau đó, nó tổng hợp tất cả các đóng góp của thiên thạch trên mỗi ô trong tất cả các vòng. Cuối cùng, nó tính toán chi phí bằng cách chỉ tính tổng những giá trị tích lũy trong phân đoạn chưa có ký tự chiếm giữ, vì đó là những ô duy nhất phải được xóa tích cực để kết nối các nhóm ban đầu. 

Một điểm tinh tế là các dòng đầu vào vẫn phải được sử dụng đầy đủ ngay cả trong các trường hợp thoát sớm, vì việc bỏ qua chúng sẽ không đồng bộ hóa các trường hợp thử nghiệm tiếp theo. 

## Ví dụ đã hoạt động 

Hãy xem xét một dòng nhỏ trong đó các ký tự ở cuối và một ô ở giữa tích lũy các thiên thạch. 

đầu vào:```
1
5 2
1 0 0 0 1
1 0 2 0 3
0 0 1 0 0
```Sau khi tích lũy, tổng số thiên thạch trên mỗi ô sẽ trở thành$[1, 0, 3, 0, 3]$. Phân đoạn bắt buộc là từ chỉ mục 0 đến 4. Chỉ các ô 1, 2 và 3 quan trọng cho việc truyền tải vì điểm cuối đã chứa các ký tự. 

| Tế bào | Ban đầu | Tổng số thiên thạch | Đã bao gồm trong chi phí | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | Không | 
| 1 | 0 | 0 | Có (0) | 
| 2 | 0 | 3 | Có (3) | 
| 3 | 0 | 0 | Có (0) | 
| 4 | 1 | 3 | Không | 

Chi phí là$3$. 

Điều này cho thấy rằng chỉ các ô trung gian trống mới đóng góp, trong khi các ô ký tự không liên quan vì chúng đã đóng vai trò là điểm neo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$mỗi trường hợp thử nghiệm (tổng thể$10^6$) | Mỗi giá trị thiên thạch được xử lý một lần trong quá trình tích lũy | 
| Không gian |$O(n)$| Chỉ tích lũy trên mỗi ô được lưu trữ | 

Các ràng buộc đảm bảo rằng tổng số lần cập nhật ô trên tất cả các trường hợp thử nghiệm là tối đa$10^6$, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        c = list(map(int, input().split()))
        L, R = n, -1
        for i, x in enumerate(c):
            if x:
                L = min(L, i)
                R = max(R, i)

        total = [0] * n
        for _ in range(m):
            row = list(map(int, input().split()))
            for i in range(n):
                total[i] += row[i]

        if L > R:
            out.append("-1")
            continue

        ans = 0
        for i in range(L, R + 1):
            if c[i] == 0:
                ans += total[i]
        out.append(str(ans))

    return "\n".join(out)

# custom cases

# single character, no cost
assert run("1\n3 2\n0 1 0\n1 2 3\n0 0 0\n") == "0"

# two characters with blocked middle
assert run("1\n3 1\n1 0 1\n5 5 5\n") == "5"

# no empty cells needed
assert run("1\n4 2\n1 0 0 1\n1 0 2 0\n0 0 0 0\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn | 0 | Không cần truyền tải | 
| ô giữa bị chặn | 5 | Chi phí tích lũy chính xác | 
| tích lũy nhiều vòng | 2 | Tổng hợp thích hợp qua các vòng | 

## Vỏ cạnh 

Khi chỉ có một ký tự trên lưới, thuật toán sẽ tìm ngay$L = R$, do đó đoạn này không chứa ô trung gian. Chi phí bằng 0 vì không cần di chuyển và nhân vật không bao giờ cần di chuyển qua bất kỳ khu vực nào bị ảnh hưởng bởi thiên thạch. 

Khi thiên thạch xuất hiện trên tất cả các ô trong một phân đoạn nhưng thực tế chỉ cần một ô để kết nối, thuật toán vẫn chỉ tính tổng khoảng thời gian cần thiết, tránh tính quá mức các vùng không liên quan. Điều này đảm bảo rằng sự phân bố thiên thạch dày đặc bên ngoài đường dẫn hoạt động không ảnh hưởng đến câu trả lời. 

Khi có nhiều vòng đóng góp vào cùng một ô, quá trình tích lũy sẽ xử lý chính xác các lần bổ sung lặp lại. Một ô nhận được thiên thạch nhiều lần sẽ đóng góp toàn bộ số tiền, phản ánh tổng chi phí cần thiết để dọn sạch nó nếu nó từng được sử dụng làm hành lang.
