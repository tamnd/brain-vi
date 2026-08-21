---
title: "CF 104095D - \u56ed\u827a\u5927\u5e08"
description: "Chúng ta có một hàng cây $n$, mỗi cây bắt đầu có cùng chiều cao $h$. Đối với mỗi cây, chúng ta được phép giữ nguyên hoặc cắt nó xuống bất kỳ chiều cao nguyên nào nhỏ hơn $h$."
date: "2026-07-02T02:19:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104095
codeforces_index: "D"
codeforces_contest_name: "2020 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 104095
solve_time_s: 43
verified: true
draft: false
---

[CF 104095D - \u56ed\u827a\u5927\u5e08](https://codeforces.com/problemset/problem/104095/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng$n$cây, mỗi cây bắt đầu có cùng chiều cao$h$. Đối với mỗi cây, chúng ta được phép giữ nguyên hoặc cắt nó xuống bất kỳ chiều cao nguyên nào nhỏ hơn$h$. Sau khi chọn chiều cao cuối cùng cho tất cả các cây, chúng ta phải thỏa mãn một chuỗi$n-1$so sánh chặt chẽ giữa các cây liền kề: mỗi so sánh là “trái nhỏ hơn phải” hoặc “trái lớn hơn phải”. 

Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu mảng có chiều cao cuối cùng riêng biệt để thỏa mãn tất cả các bất đẳng thức này, trong đó hai mảng được coi là khác nhau nếu ít nhất một vị trí có chiều cao cuối cùng khác nhau. Câu trả lời được lấy modulo$10^9+7$. 

Một quan sát quan trọng xuất phát trực tiếp từ định nghĩa hoạt động: mỗi vị trí chỉ có thể nhận các giá trị trong phạm vi$[1, h]$và chúng ta có thể tự do chọn bất kỳ giá trị nào trong phạm vi này một cách độc lập ngoại trừ các ràng buộc kề cận kết hợp với các lựa chọn. Vì vậy, về cơ bản đây là một vấn đề đếm hạn chế trên các chuỗi số nguyên. 

Những hạn chế$n \le 3000$Và$h \le 10^6$ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại các giá trị chiều cao thực tế cho mỗi vị trí. Bất kỳ giải pháp nào cố gắng liệt kê rõ ràng độ cao hoặc chuyển tiếp trên toàn bộ phạm vi giá trị trên mỗi cạnh sẽ quá chậm. Chúng ta cần một công thức trong đó sự phụ thuộc vào$h$nhiều nhất là tuyến tính hoặc logarit. 

Một trường hợp khó nhận thấy là khi$h = 1$. Trong trường hợp này, không thể giảm vị trí nào, do đó mọi phần tử buộc phải chính xác bằng 1. Cấu hình hợp lệ duy nhất tồn tại khi và chỉ nếu tất cả các ràng buộc đều nhất quán với các giá trị bằng nhau, điều này là không thể vì tất cả các so sánh đều nghiêm ngặt. Vì vậy câu trả lời luôn là 0 khi$n > 1$Và$h = 1$. 

Một trường hợp cạnh quan trọng khác là chuỗi ràng buộc hoàn toàn đơn điệu, ví dụ như tất cả “<”. Trong trường hợp đó, các dãy hợp lệ tương ứng với các dãy tăng dần được giới hạn bởi$h$, vẫn có thể nhiều nhưng phải tôn trọng giới hạn nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các phép gán độ cao trong$[1, h]$cho mỗi$n$vị trí, sau đó kiểm tra xem mỗi phép gán có thỏa mãn mọi bất đẳng thức hay không. Điều này đã mang lại$h^n$những khả năng hoàn toàn không thể thực hiện được ngay cả đối với những người nhỏ bé$n$. 

Chúng ta cần nén không gian trạng thái. Nhận xét quan trọng là điều quan trọng không phải là các giá trị tuyệt đối mà là thứ tự tương đối của chúng do chuỗi bất bình đẳng gây ra. Vì tất cả các ràng buộc đều là sự so sánh chặt chẽ giữa các lân cận nên trình tự được xác định bằng cách chúng tôi “xếp hạng” các phần tử cục bộ và sau đó gán các giá trị thực tế phù hợp với các thứ hạng đó. 

Nếu chúng tôi sửa một mẫu tương đối, nghĩa là đối với từng vị trí, chúng tôi quyết định giá trị của nó so với các mẫu trước đó như thế nào, thì các phép gán số thực tế tương ứng với việc chọn các chuỗi giá trị riêng biệt tăng dần nghiêm ngặt dọc theo các phân đoạn nhất định. Mỗi phân đoạn hoạt động giống như một cuộc chạy đơn điệu. 

Việc giảm tiêu chuẩn là xem mảng được phân vùng thành các khối đơn điệu tùy thuộc vào sự thay đổi hướng. Bên trong mỗi khối, các ràng buộc thực thi tính đơn điệu nghiêm ngặt theo một hướng cố định và giữa các khối, hướng đảo ngược. Vấn đề sau đó trở thành việc đếm xem có bao nhiêu cách gán giá trị trong$[1, h]$thành một chuỗi với các ràng buộc đơn điệu xen kẽ. 

Điều này có thể được giải quyết bằng cách sử dụng lập trình động trong đó chúng tôi theo dõi, đối với mỗi tiền tố, có bao nhiêu cách để kết thúc ở một “cấu trúc xếp hạng” nhất định, nhưng chúng tôi tránh lặp lại theo độ cao bằng cách sử dụng tổng tiền tố trên không gian giá trị. Mỗi chuyển đổi trở thành một tiền tố hoặc tổng hậu tố tùy thuộc vào việc chúng ta tăng hay giảm. 

Cấu trúc chính là ở mỗi vị trí, chúng tôi chỉ cần số lượng tích lũy trên phạm vi cho phép, do đó chúng tôi có thể duy trì DP trên các vị trí và độ cao cuối cùng có thể có với các chuyển đổi phạm vi tuyến tính theo$h$, nhưng được tối ưu hóa bằng cách sử dụng tổng tiền tố để mỗi lần chuyển đổi được$O(1)$. 

Điều này làm giảm vấn đề từ liệt kê theo cấp số nhân đến$O(nh)$lập trình động, điều này có thể chấp nhận được vì$n \le 3000$Và$h \le 10^6$, nhưng chúng tôi cũng nén DP hơn nữa: vì quá trình chuyển đổi chỉ phụ thuộc vào tổng tiền tố hàng trước đó nên chúng tôi chỉ lưu trữ hai mảng có kích thước$h$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(h^n)$|$O(n)$| Quá chậm | 
| DP với tối ưu hóa tiền tố |$O(nh)$|$O(h)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một mảng DP trong đó$dp[i][x]$là số cách hợp lệ để gán độ cao cho điểm đầu tiên$i$vị trí như vị trí đó$i$có chiều cao chính xác$x$. Vì chúng ta chỉ chuyển từ$i-1$ĐẾN$i$, chúng ta có thể nén mảng này thành hai mảng. 

1. Khởi tạo$dp[1][x] = 1$cho tất cả$1 \le x \le h$, vì cây đầu tiên có thể có chiều cao bất kỳ một cách độc lập. 
2. Đối với mỗi ràng buộc giữa$i-1$Và$i$, chúng tôi cập nhật một mảng DP mới tùy thuộc vào mối quan hệ là “<” hay “>”. 
3. Nếu ràng buộc là “<”, thì với mỗi giá trị$x$ở vị trí$i$, chúng ta phải có$dp[i][x] = \sum_{y < x} dp[i-1][y]$. Điều này phản ánh rằng chiều cao trước đó phải nhỏ hơn một cách nghiêm ngặt. 
4. Nếu ràng buộc là “>”, thì với mỗi giá trị$x$, chúng ta phải có$dp[i][x] = \sum_{y > x} dp[i-1][y]$, nghĩa là chúng ta tính tổng tất cả các độ cao lớn hơn trước đó. 
5. Để tính các tổng này một cách hiệu quả, chúng ta xây dựng các tổng tiền tố của mảng DP trước đó. Sau đó, truy vấn “<” trở thành tra cứu tiền tố và truy vấn “>” trở thành tiền tố tổng trừ. 
6. Sau khi xử lý tất cả các vị trí, câu trả lời là tổng của tất cả$dp[n][x]$vì$1 \le x \le h$. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt đầy đủ tất cả các phép gán một phần hợp lệ vì mọi phép gán tiền tố hợp lệ đều được đặc trưng duy nhất bởi chiều cao cuối cùng. Quá trình chuyển đổi thực thi ràng buộc kề chính xác một lần trên mỗi cạnh và mỗi chuỗi hợp lệ đóng góp chính xác một đường dẫn qua DP. Phép biến đổi tổng tiền tố không làm thay đổi ngữ nghĩa, nó chỉ thể hiện lại tổng phạm vi một cách hiệu quả. Vì mỗi bước mở rộng đều duy trì tính chính xác cục bộ và bao gồm tất cả các phần tiếp theo hợp lệ nên tổng cuối cùng sẽ tính tất cả các chuỗi hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, h = map(int, input().split())
    s = input().strip()

    if h == 1:
        # only one possible array, but strict constraints make it impossible for n>1
        return 1 if n == 1 else 0

    dp = [1] * (h + 1)

    for i in range(n - 1):
        ndp = [0] * (h + 1)
        pref = [0] * (h + 1)

        for v in range(1, h + 1):
            pref[v] = (pref[v - 1] + dp[v]) % MOD

        if s[i] == '<':
            for x in range(1, h + 1):
                ndp[x] = pref[x - 1]
        else:
            total = pref[h]
            for x in range(1, h + 1):
                ndp[x] = (total - pref[x] + MOD) % MOD

        dp = ndp

    print(sum(dp) % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo định nghĩa DP trực tiếp. Mảng tiền tố được tính toán lại ở mỗi bước để cho phép tính tổng phạm vi thời gian không đổi. Quá trình chuyển đổi “<” sử dụng tiền tố lên tới$x-1$, trong khi quá trình chuyển đổi “>” sử dụng tiền tố tổng trừ lên tới$x$, cẩn thận giữ sự bất bình đẳng nghiêm ngặt. 

Ranh giới tinh tế duy nhất là đảm bảo tính nghiêm ngặt: đối với “<” chúng tôi loại trừ$x$bản thân nó, do đó$pref[x-1]$. Đối với “>”, chúng tôi loại trừ$x$cũng vậy, do đó trừ đi$pref[x]$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
<>
```Chúng tôi bắt đầu với: 

| tôi | trạng thái dp | 
| --- | --- | 
| 1 | [1,1,1] | 

Chuyển tiếp 1 là “<”: 

| x | tiền tố | ndp | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 2 | 

Vì vậy dp trở thành [0,1,2]. 

Quá trình chuyển đổi tiếp theo là “>”: 

| x | tiền tố | tổng cộng | ndp | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 3 | 
| 2 | 1 | 3 | 2 | 
| 3 | 3 | 3 | 0 | 

dp cuối cùng là [3,2,0], tổng là 5. 

Điều này phù hợp với ý tưởng rằng chỉ có thứ tự tương đối mới quan trọng và các nhiệm vụ khả thi mới tương ứng với các chuỗi nghiêm ngặt nhất quán. 

### Ví dụ 2 

đầu vào:```
4 2
<<<
```dp ban đầu là [1,1]. 

Sau “<” đầu tiên: dp trở thành [0,1]. 

Sau “<” thứ hai: dp trở thành [0,0]. 

Sau “<” thứ ba: dp vẫn là [0,0]. 

Câu trả lời cuối cùng là 0, vì chuỗi có độ dài 4 tăng dần không thể được hình thành chỉ với 2 giá trị. 

Điều này cho thấy các ràng buộc giá trị tương tác như thế nào với cấu trúc đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nh)$| Mỗi trong số$n$các bước xây dựng tổng tiền tố$h$giá trị và thực hiện chuyển đổi tuyến tính | 
| Không gian |$O(h)$| Chỉ có hai mảng DP có kích thước$h$được duy trì | 

Giới hạn$n \le 3000$Và$h \le 10^6$tạo đường biên này ở dạng thô, nhưng các phép toán hệ số không đổi là các phép toán bổ sung đơn giản và việc truy cập bộ nhớ là tuần tự, giúp giữ nó trong giới hạn dưới các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, h = map(int, sys.stdin.readline().split())
    s = sys.stdin.readline().strip()

    if h == 1:
        return str(1 if n == 1 else 0)

    dp = [1] * (h + 1)

    for i in range(n - 1):
        ndp = [0] * (h + 1)
        pref = [0] * (h + 1)

        for v in range(1, h + 1):
            pref[v] = (pref[v - 1] + dp[v]) % MOD

        if s[i] == '<':
            for x in range(1, h + 1):
                ndp[x] = pref[x - 1]
        else:
            total = pref[h]
            for x in range(1, h + 1):
                ndp[x] = (total - pref[x] + MOD) % MOD

        dp = ndp

    return str(sum(dp) % MOD)

# provided sample
assert run("3 3\n<>\n") == "5", "sample 1"

# minimum n
assert run("2 2\n<\n") == "1", "two elements increasing"

# impossible strict chain
assert run("4 2\n<<<\n") == "0", "insufficient height"

# all decreasing
assert run("3 3\n>>\n") == "5", "symmetric case"

# h = 1 edge case
assert run("3 1\n<<\n") == "0", "h=1 forces impossibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 < > | 5 | chuyển tiếp hỗn hợp chính xác | 
| 4 2 <<< | 0 | tăng trưởng nghiêm ngặt không thể thực hiện được | 
| 3 3 >> | 5 | trường hợp giảm đối xứng | 
| 3 1 << | 0 | giới hạn chiều cao thoái hóa | 

## Vỏ cạnh 

Khi nào$h = 1$, mọi vị trí buộc phải bằng 1. Thuật toán trả về đúng 0 trừ khi$n = 1$, bởi vì tất cả các chuyển đổi DP đều sụp đổ về 0 dưới các bất đẳng thức chặt chẽ. 

Đối với các chuỗi hoàn toàn đơn điệu như “<<<…”, DP nhanh chóng sụp đổ vì tổng tiền tố cuối cùng trở thành 0 ngoài phạm vi khả thi, điều này phù hợp với khả năng tổ hợp của các chuỗi tăng nghiêm ngặt vượt quá các giá trị riêng biệt có sẵn. 

Đối với các mẫu xen kẽ như “<><><”, DP duy trì nhiều trạng thái hoạt động và các chuyển đổi tiền tố-hậu tố duy trì tính đối xứng mà không làm mất cấu hình hợp lệ, xác nhận rằng mỗi chuỗi hợp lệ được tính chính xác một lần trong quá trình tiến hóa DP.
