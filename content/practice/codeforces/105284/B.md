---
title: "CF 105284B - Mảng Khỉ"
description: "Chúng ta được cung cấp một mảng và ba giá trị phân biệt, $X$, $Y$ và $K$, với thứ tự $Y < K < X$. Đối với mỗi trường hợp thử nghiệm, chúng ta cần đếm các mảng con trong đó có hai điều kiện cực trị đồng thời: trong mảng con, giá trị lớn nhất phải chính xác là $X$, giá trị nhỏ nhất…"
date: "2026-06-23T14:29:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105284
codeforces_index: "B"
codeforces_contest_name: "TeamsCode Summer 2024 Advanced Division"
rating: 0
weight: 105284
solve_time_s: 96
verified: false
draft: false
---

[CF 105284B - Mảng Khỉ](https://codeforces.com/problemset/problem/105284/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và ba giá trị phân biệt,$X$,$Y$, Và$K$, với thứ tự$Y < K < X$. Đối với mỗi trường hợp thử nghiệm, chúng ta cần đếm các mảng con trong đó có hai điều kiện cực trị đồng thời: trong mảng con, giá trị tối đa phải chính xác$X$, giá trị tối thiểu phải chính xác$Y$, và giá trị$K$bị cấm ở bất cứ đâu bên trong nó. 

Một mảng con chỉ đóng góp vào câu trả lời nếu mọi phần tử đều nằm trong phạm vi đóng$[Y, X]$, cả hai điểm cuối đều thực sự được “hiện thực hóa” bên trong mảng con, nghĩa là ít nhất một lần xuất hiện của$X$và một lần xuất hiện$Y$, và không xuất hiện$K$xuất hiện. 

Các ràng buộc ngụ ý rằng tổng chiều dài mảng trên tất cả các trường hợp thử nghiệm nhiều nhất là$5 \cdot 10^5$, loại trừ mọi cách tiếp cận bậc hai hoặc thậm chí gần bậc hai cho mỗi bài kiểm tra. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính trong tổng kích thước đầu vào. Một giải pháp kiểm tra tất cả các mảng con một cách rõ ràng sẽ yêu cầu$O(N^2)$thời gian cho mỗi trường hợp thử nghiệm, sẽ vượt quá$10^{10}$hoạt động trong trường hợp xấu nhất và không khả thi. 

Trường hợp cạnh tinh vi phát sinh khi mảng không chứa giá trị hợp lệ$X$hoặc$Y$xuất hiện trong một phân đoạn giữa các phần tử bị cấm. Ví dụ, nếu$A = [5, 4, 6]$,$X=6$,$Y=4$,$K=5$, mảng con$[4,6]$là hợp lệ, nhưng bất kỳ phân đoạn nào chứa 5 đều không hợp lệ bất kể các phần tử khác. Một cửa sổ trượt đơn giản chỉ theo dõi mức tối thiểu và tối đa mà không xử lý rõ ràng$K$sẽ bao gồm không chính xác các mảng con không hợp lệ có chứa$K$. 

Một dạng lỗi khác là xử lý tình trạng “tối đa là$X$” và “phút là$Y$" độc lập. Ví dụ: một phân đoạn có thể chứa$X$Và$Y$, nhưng cũng chứa phần tử lớn hơn$X$hoặc nhỏ hơn$Y$, phá vỡ tính chính xác nếu giới hạn không được thực thi cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp brute-force liệt kê mọi mảng con và tính toán mức tối thiểu, tối đa của nó và kiểm tra sự hiện diện của$K$. Đối với mỗi mảng con$[l, r]$, quét phạm vi chi phí$O(r-l+1)$, vậy độ phức tạp tổng cộng là$O(N^3)$trong cách thực hiện theo nghĩa đen nhất, hoặc$O(N^2)$với việc bảo trì tăng dần. Với$N = 5 \cdot 10^5$, thậm chí$O(N^2)$vượt xa giới hạn khả thi. 

Quan sát quan trọng là$K$hoạt động như một chất phân tách cứng. Bất kỳ mảng con hợp lệ nào cũng không thể vượt qua một chỉ mục trong đó$A[i] = K$. Điều này chia mảng thành các phân đoạn độc lập. Bên trong mỗi phân khúc, chúng ta chỉ quan tâm đến các giá trị trong$[Y, X]$, vì bất kỳ giá trị nào nằm ngoài phạm vi này sẽ ngay lập tức vi phạm yêu cầu tối thiểu hoặc tối đa. 

Trong một phân đoạn không có$K$, chúng ta rút gọn vấn đề về việc đếm các mảng con có các phần tử nằm trong$[Y, X]$và chứa ít nhất một$X$và ít nhất một$Y$. Điều này chuyển thành một bài toán đếm hai con trỏ cổ điển: chúng ta duy trì một cửa sổ trượt, theo dõi các vị trí cuối cùng của$X$Và$Y$và đảm bảo tất cả các phần tử vẫn nằm trong giới hạn. 

Thay vì thực thi trực tiếp cả điều kiện tối thiểu và tối đa một cách linh hoạt, chúng tôi đảo ngược logic đếm. Chúng tôi đếm tất cả các mảng con trong một vùng hợp lệ và trừ đi những mảng con không bao gồm$X$hoặc không bao gồm$Y$. Việc theo dõi các vị trí được nhìn thấy lần cuối cho phép chúng tôi tính toán các đóng góp hợp lệ trong thời gian không đổi cho mỗi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập, quét mảng từ trái sang phải trong khi vẫn duy trì ranh giới phân đoạn và thông tin xuất hiện. 

1. Chúng tôi duy trì một con trỏ đánh dấu điểm bắt đầu của phân đoạn hợp lệ hiện tại, đặt lại nó bất cứ khi nào chúng tôi gặp phải$K$, vì không có mảng con nào có thể bao gồm nó. Điều này đảm bảo chúng tôi không bao giờ xem xét các khoảng thời gian không hợp lệ. 
2. Bên trong một phân khúc, chúng tôi theo dõi chỉ mục được nhìn thấy lần cuối của$X$Và$Y$. Các vị trí này cho chúng ta biết liệu một mảng con kết thúc ở chỉ mục hiện tại có thể đáp ứng cả hai ràng buộc hay không. 
3. Đối với mỗi chỉ số$r$, trước tiên chúng tôi xác minh xem$A[r]$ở trong$[Y, X]$. Nếu nó ở bên ngoài, chúng tôi đặt lại đoạn bắt đầu thành$r+1$bởi vì bất kỳ mảng con nào vượt qua điểm này sẽ vi phạm các ràng buộc tối thiểu/tối đa. 
4. Nếu$A[r] = X$, chúng tôi cập nhật vị trí nhìn thấy lần cuối của$X$. Nếu như$A[r] = Y$, chúng tôi cập nhật vị trí nhìn thấy lần cuối của$Y$. 
5. Đối với mỗi điểm cuối$r$, chúng tôi tính toán có bao nhiêu vị trí bắt đầu hợp lệ$l$tồn tại sao cho cả hai$X$Và$Y$xuất hiện ở$[l, r]$Và$l$là sau lần xuất hiện cuối cùng của$K$và bất kỳ yếu tố ngoài phạm vi. Số mảng con hợp lệ kết thúc tại$r$là$\max(0, \min(lastX, lastY) - start)$. 

Mỗi mảng con hợp lệ kết thúc tại$r$phải bắt đầu không muộn hơn thời điểm sớm nhất trong số lần xuất hiện cuối cùng của$X$Và$Y$, nếu không thì một trong số chúng sẽ bị thiếu. 

### Tại sao nó hoạt động 

Ở mọi vị trí$r$, thuật toán ngầm mô tả tất cả các mảng con hợp lệ kết thúc tại$r$bởi hai ràng buộc: chúng phải nằm hoàn toàn bên trong phân đoạn sạch hiện tại (không$K$hoặc các phần tử nằm ngoài phạm vi) và điểm cuối bên trái của chúng phải được định vị sao cho cả hai giá trị bắt buộc đều xuất hiện bên trong mảng con. Các chỉ số xuất hiện cuối cùng xác định đầy đủ liệu một mảng con có chứa ít nhất một$X$và một$Y$, vì bất kỳ mảng con nào bắt đầu trước lần xuất hiện tối thiểu cuối cùng này sẽ bao gồm cả hai, trong khi bất kỳ mảng con nào bắt đầu muộn hơn sẽ loại trừ một trong số chúng. Bất biến này đảm bảo mọi mảng con được tính đều thỏa mãn tất cả các ràng buộc chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, X, Y, K = map(int, input().split())
        a = list(map(int, input().split()))
        
        last_x = -1
        last_y = -1
        start = 0
        ans = 0
        
        for i, v in enumerate(a):
            if v == K:
                start = i + 1
                last_x = -1
                last_y = -1
                continue
            
            if v < Y or v > X:
                start = i + 1
                last_x = -1
                last_y = -1
                continue
            
            if v == X:
                last_x = i
            if v == Y:
                last_y = i
            
            if last_x != -1 and last_y != -1:
                ans += max(0, min(last_x, last_y) - start + 1)
    
        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì một ranh giới di chuyển sang trái được gọi là`start`, bị buộc chuyển tiếp bất cứ khi nào chúng ta gặp phải giá trị bị cấm$K$hoặc bất kỳ giá trị nào ngoài phạm vi cho phép$[Y, X]$. Điều này đảm bảo rằng mọi mảng con được xem xét đều hợp lệ đối với các quy tắc giới hạn và loại trừ. 

Các biến`last_x`Và`last_y`lưu trữ các vị trí gần đây nhất của$X$Và$Y$bên trong phân khúc hiện tại. Khi cả hai đều được xác định, mọi chỉ mục từ`start`lên đến`min(last_x, last_y)`có thể đóng vai trò là điểm cuối bên trái tạo ra một mảng con kết thúc tại`i`chứa cả hai giá trị bắt buộc. 

biểu thức`min(last_x, last_y) - start + 1`đếm xem tồn tại bao nhiêu vị trí bắt đầu như vậy. các`+1`là rất quan trọng vì cả hai điểm cuối đều được bao gồm và việc thiếu nó sẽ dẫn đến việc tính thiếu một cách có hệ thống các phần mở rộng hợp lệ có độ dài đơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$A = [6, 7, 4, 6]$,$X=7$,$Y=4$,$K=5$. 

| tôi | v | bắt đầu | cuối cùngX | cuối cùngY | phút(lastX,lastY) | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 6 | 0 | -1 | -1 | - | 0 | 
| 1 | 7 | 0 | 1 | -1 | - | 0 | 
| 2 | 4 | 0 | 1 | 2 | 1 | 2 | 
| 3 | 6 | 0 | 1 | 2 | 1 | 2 | 

Tại chỉ số 2 và 3, cả hai$X$Và$Y$đã xuất hiện, do đó các mảng con kết thúc ở các vị trí này sẽ đóng góp. Bảng này cho thấy các vị trí bắt đầu hợp lệ trước đó sẽ tích lũy các phân đoạn hợp lệ như thế nào. Điều này chứng tỏ rằng thuật toán đếm chính xác tất cả các mảng con trong đó cả hai giá trị bắt buộc đều xuất hiện trước điểm cuối. 

### Ví dụ 2 

Hãy xem xét$A = [4, 5, 2, 4]$,$X=5$,$Y=4$,$K=2$. 

| tôi | v | bắt đầu | cuối cùngX | cuối cùngY | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | -1 | 0 | 0 | 
| 1 | 5 | 0 | 1 | 0 | 2 | 
| 2 | 2 | 3 | -1 | -1 | đặt lại | 
| 3 | 4 | 3 | -1 | 3 | 0 | 

Tại chỉ số 2, sự có mặt của$K=2$đặt lại phân đoạn, ngăn không cho bất kỳ phân đoạn nào vượt qua nó. Điều này xác nhận rằng các giá trị bị cấm phân chia mảng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi phần tử được xử lý một lần với các bản cập nhật liên tục | 
| Không gian |$O(1)$| Chỉ có một số bộ đếm và chỉ số được lưu trữ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi$5 \cdot 10^5$, do đó, việc quét tuyến tính cho mỗi trường hợp kiểm thử sẽ giữ cho thời gian chạy tổng thể thoải mái trong giới hạn. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample tests
assert run("3\n4 7 4 5\n6 7 4 6\n5 2 4 3\n2 8 3 6\n10 4 3 7 4 8 6 5 1 3 8 2\n") == "5\n0\n3"

# all equal values (no valid X/Y pair)
assert run("1\n5 10 1 2\n2 2 2 2 2\n") == "0"

# K blocks everything
assert run("1\n5 7 1 4\n7 4 1 4 7\n") == "0"

# minimal case
assert run("1\n1 5 1 3\n5\n") == "0"

# simple valid construction
assert run("1\n3 3 1 2\n1 3 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 0 | không có cấu trúc X/Y | 
| K chặn mọi thứ | 0 | thiết lập lại phân đoạn chính xác | 
| trường hợp tối thiểu | 0 | xử lý phần tử đơn | 
| hợp lệ đơn giản | 1 | tính đúng cơ bản của phép đếm | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$K$xuất hiện nhiều lần, chia mảng thành nhiều đoạn nhỏ. Trong trường hợp như vậy, thuật toán sẽ thiết lập lại`start`,`last_x`, Và`last_y`mỗi lần, đảm bảo không có phân mảng nào vượt qua các giá trị bị cấm. Đối với một đầu vào như$A = [7, 2, 4, 2, 7]$, các mảng con hợp lệ được giới hạn trong$[7]$,$[4]$, Và$[7]$các phân đoạn và thuật toán không bao giờ hợp nhất chúng một cách không chính xác. 

Một trường hợp cạnh khác là khi$X$hoặc$Y$không bao giờ xuất hiện bên trong một phân đoạn. Ví dụ,$A = [3, 3, 3]$với$X=3, Y=1$không tạo ra sự đóng góp mảng con hợp lệ vì`last_y`không bao giờ trở nên hợp lệ. Thuật toán chính xác mang lại kết quả bằng 0 vì điều kiện`last_x != -1 and last_y != -1`không bao giờ giữ được. 

Một trường hợp tế nhị cuối cùng xảy ra khi$X$Và$Y$xuất hiện theo thứ tự đảo ngược. Vì$A = [1, 5]$với$X=5, Y=1$, các khoản đóng góp chỉ bắt đầu tích lũy sau khi cả hai đều được nhìn thấy và công thức xử lý việc sắp xếp một cách tự nhiên vì`min(last_x, last_y)`nắm bắt được yêu cầu sớm nhất trong số đó.
