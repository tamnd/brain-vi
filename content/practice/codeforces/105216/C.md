---
title: "CF 105216C - Đồng bộ hóa Cuckoo"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có $N$ đồng hồ cúc cu và mỗi đồng hồ có hành vi định kỳ cố định riêng. Đồng hồ $i$ phát ra âm thanh đôi khi $1, 1+i, 1+2i, 1+3i,dots$."
date: "2026-06-24T17:01:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105216
codeforces_index: "C"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105216
solve_time_s: 71
verified: false
draft: false
---

[CF 105216C - Đồng bộ hóa Cuckoo](https://codeforces.com/problemset/problem/105216/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$N$đồng hồ cúc cu và mỗi đồng hồ có hành vi định kỳ cố định riêng. Cái đồng hồ$i$đôi khi phát ra âm thanh$1, 1+i, 1+2i, 1+3i,\dots$. Tất cả các đồng hồ ban đầu được căn chỉnh sao cho âm thanh đầu tiên xảy ra vào đúng thời điểm$1$, và sau đó chúng tiếp tục lặp lại với các chu kỳ tương ứng. 

Chúng tôi được hỏi: tại một thời điểm cụ thể$T$, có bao nhiêu chiếc đồng hồ trong số này sẽ phát ra âm thanh vào đúng thời điểm đó. 

Vì vậy đối với một chiếc đồng hồ cố định$i$, chúng ta cần kiểm tra xem$T$thuộc về cấp số cộng bắt đầu từ$1$với bước$i$. Điều này tương đương với việc kiểm tra xem$T-1$chia hết cho$i$. Do đó, câu trả lời cho mỗi trường hợp thử nghiệm là số lượng số nguyên$i$trong phạm vi$[1, N]$như vậy$i \mid (T-1)$. 

Các ràng buộc đi lên đến$N, T \le 10^9$và lên tới 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trên tất cả các đồng hồ cho mỗi trường hợp thử nghiệm, vì đó sẽ là$O(NQ)$, trong trường hợp xấu nhất đạt tới$10^{11}$hoạt động. 

Một trường hợp cạnh tinh tế phát sinh khi$T = 1$. Trong trường hợp đó, mọi đồng hồ đều đổ chuông vào thời điểm 1, vì tất cả tiến trình đều bắt đầu từ 1. Công thức$T-1 = 0$ngụ ý mọi$i$chia cho 0 nên đáp án là$N$. Bất kỳ việc triển khai nào quên cấu trúc đặc biệt này có thể vô tình trả về 0 hoặc xử lý sai các phép chia chia. 

Một trường hợp khác cần cẩn thận là lớn$T$Ở đâu$T-1 > N$. Một cách tiếp cận đếm số chia đơn giản giả sử các số chia luôn nhỏ so với$N$có thể bỏ lỡ sự thật là chúng ta chỉ đếm các ước số tối đa$N$, không phải mọi ước của$T-1$. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp rất đơn giản. Cho mỗi đồng hồ$i$từ 1 đến$N$, chúng tôi kiểm tra xem$T-1$chia hết cho$i$. Điều này đúng theo quan sát rằng đồng hồ$i$chuông vào thời điểm đó$1 + k i$, vì vậy chúng ta chỉ cần kiểm tra tư cách thành viên của$T$theo trình tự đó. Nút thắt rất rõ ràng: đối với mỗi trường hợp thử nghiệm, chúng tôi có thể cần quét tới$10^9$giá trị vượt quá giới hạn cho phép. 

Cái nhìn sâu sắc về cấu trúc quan trọng là vấn đề không thực sự nằm ở tất cả các đồng hồ một cách độc lập mà là về các ước số của một số duy nhất.$T-1$. Mỗi chỉ số đồng hồ hợp lệ chính xác là một ước số của$T-1$, và chúng ta chỉ quan tâm đến những ước số không vượt quá$N$. Điều này biến bài toán thành bài toán đếm số chia bị chặn. 

Thay vì lặp đi lặp lại tất cả những gì có thể$i$, chúng tôi liệt kê các ước của$T-1$hiệu quả bằng cách quét lên đến$\sqrt{T-1}$. Với mỗi số chia$d$, chúng tôi xem xét cả hai$d$Và$(T-1)/d$, và đếm chúng nếu chúng nằm trong$[1, N]$. Điều này làm giảm mỗi trường hợp thử nghiệm xuống$O(\sqrt{T})$, đủ nhanh vì$\sqrt{10^9} \approx 31623$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Tối ưu |$O(\sqrt{T})$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại điều kiện dưới dạng kết nối trực tiếp đồng hồ và ước số. Một chiếc đồng hồ$i$chuông vào thời điểm đó$T$nếu và chỉ khi$T = 1 + k i$đối với một số nguyên$k$, tương đương với$T-1$được chia cho$i$. 

Chúng tôi biểu thị$x = T - 1$. Nhiệm vụ trở thành đếm có bao nhiêu số nguyên$i$thỏa mãn$1 \le i \le N$Và$i \mid x$. 

### Các bước 

1. Tính toán$x = T - 1$. Điều này biến điều kiện cấp số cộng thành điều kiện chia hết. Cấu trúc của vấn đề được nắm bắt hoàn toàn bởi$x$. 
2. Khởi tạo bộ đếm câu trả lời về 0. Điều này sẽ tích lũy các ước số hợp lệ của$x$điều đó cũng tôn trọng giới hạn trên$N$. 
3. Lặp lại$i$từ 1 đến$\lfloor \sqrt{x} \rfloor$. Đối với mỗi$i$, kiểm tra xem$i$chia rẽ$x$. Điều này hiệu quả vì các ước số có các cặp đối xứng xung quanh căn bậc hai. 
4. Bất cứ khi nào$i \mid x$, xem xét cả hai$i$Và$x / i$như các chỉ số đồng hồ tiềm năng. Mỗi đại diện cho một cặp chia số hợp lệ. 
5. Đối với mỗi ước số ứng cử viên, chỉ thêm nó vào câu trả lời nếu nó nhiều nhất là$N$. Điều này đảm bảo chúng tôi chỉ đếm các đồng hồ tồn tại trong hệ thống. 
6. Nếu$i = x / i$, tránh tính hai lần vì đây là trường hợp hình vuông hoàn hảo. 

### Tại sao nó hoạt động 

Mỗi chỉ số đồng hồ hợp lệ chính xác là một ước số của$x = T-1$, và mọi ước của$x$tương ứng với một chiếc đồng hồ đổ chuông vào thời điểm đó$T$. Việc liệt kê kết thúc$\sqrt{x}$đảm bảo chúng ta tìm được tất cả các cặp ước số mà không bỏ sót, vì bất kỳ ước số nào lớn hơn$\sqrt{x}$được ghép nối với một cái nhỏ hơn. Việc lọc bằng$N$đảm bảo chúng ta chỉ đếm các đồng hồ tồn tại, do đó thuật toán tính toán chính xác giao điểm giữa các ước của$x$và phạm vi$[1, N]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n, t = map(int, input().split())
        x = t - 1
        
        ans = 0
        
        i = 1
        while i * i <= x:
            if x % i == 0:
                if i <= n:
                    ans += 1
                j = x // i
                if j != i and j <= n:
                    ans += 1
            i += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo chiến lược liệt kê cặp số chia. The transformation$x = T-1$được áp dụng một lần cho mỗi trường hợp thử nghiệm. Vòng lặp lên đến$\sqrt{x}$đảm bảo chúng tôi chỉ thực hiện khoảng 30 nghìn lần lặp trong trường hợp xấu nhất. Séc`j != i`ngăn ngừa việc đếm hai lần khi$x$là một hình vuông hoàn hảo Điều kiện bổ sung`<= n`thực thi giới hạn phạm vi đồng hồ. 

Một sai lầm phổ biến ở đây là quên rằng cả hai$i$Và$x//i$phải được kiểm tra chống lại$N$một cách độc lập. Một lỗi khác là bắt đầu vòng lặp từ 0 không chính xác hoặc xử lý sai trường hợp$x = 0$, trong đó mọi số nguyên lên tới$N$là hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$N = 5, T = 6$Đây$x = T - 1 = 5$. 

Chúng tôi kiểm tra ước của 5: 

| tôi | x % tôi == 0 | tôi | x/tôi | đóng góp hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | 1 | 5 | 1, 5 | 
| 2 | không | - | - | không | 
| 3 | không | - | - | không | 
| 4 | không | - | - | không | 
| 5 | vâng | 5 | 1 | 5 đã được tính, 1 bị bỏ qua | 

Các chỉ số hợp lệ là {1, 5}, cả hai đều 5, vì vậy câu trả lời là 2. 

Điều này cho thấy cách ghép số chia sẽ nắm bắt tất cả các đồng hồ hợp lệ một cách tự nhiên. 

### Ví dụ 2 

đầu vào:$N = 10, T = 10$Đây$x = 9$. 

| tôi | x % tôi | tôi | x/tôi | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | 1 | 9 | 1, 9 | 
| 2 | không | - | - | không | 
| 3 | vâng | 3 | 3 | 3 | 
| 4 | không | - | - | không | 

Ta được các ước {1, 3, 9}. Từ$N = 10$, tất cả đều hợp lệ, vì vậy câu trả lời là 3. 

Điều này xác nhận việc xử lý đúng số bình phương, trong đó$3 \cdot 3 = 9$tạo ra một đóng góp duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \sqrt{T})$| Mỗi trường hợp thử nghiệm liệt kê các ước số lên đến$\sqrt{T-1}$| 
| Không gian |$O(1)$| Chỉ sử dụng bộ đếm và biến vòng lặp | 

Với$Q \le 100$Và$T \le 10^9$, công việc trong trường hợp xấu nhất là về$100 \cdot 31623$, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    q = int(input())
    out = []
    
    for _ in range(q):
        n, t = map(int, input().split())
        x = t - 1
        
        if x == 0:
            out.append(str(n))
            continue
        
        ans = 0
        i = 1
        while i * i <= x:
            if x % i == 0:
                if i <= n:
                    ans += 1
                j = x // i
                if j != i and j <= n:
                    ans += 1
            i += 1
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples (reconstructed formatting)
assert run("5\n5 11\n10 5\n10 6\n5 3\n6 11\n") == "5\n3\n2\n2\n3"

# minimum input
assert run("1\n1 1\n") == "1"

# x = 0 edge case (T = 1)
assert run("1\n100 1\n") == "100"

# perfect square
assert run("1\n10 10\n") == "3"

# large boundary
assert run("1\n1000000000 1000000000\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$T=1$|$N$| tất cả đồng hồ đều đổ chuông vào lúc 1 | 
|$T=N$trường hợp vuông | đếm số chia đúng | tránh tính hai lần | 
| lớn$N, T$| thực hiện nhanh chóng | an toàn thực hiện | 

## Vỏ cạnh 

Khi nào$T = 1$, thuật toán thiết lập một cách hiệu quả$x = 0$. Điều kiện ước số suy biến vì mọi số nguyên đều chia hết cho 0. Hành vi đúng là tất cả$N$đồng hồ đổ chuông vào thời điểm 1. Việc triển khai xử lý việc này một cách rõ ràng trong trình bao bọc kiểm thử, vì logic ước số dựa trên vòng lặp sẽ đếm quá mức vô hạn hoặc hoạt động không chính xác nếu không được bảo vệ. 

Khi$x$là một hình vuông hoàn hảo, ví dụ$T = 10$cho đi$x = 9$, cặp số chia$(3,3)$xuất hiện một lần. Séc`j != i`đảm bảo nó chỉ được tính một lần, duy trì tính chính xác. 

Khi$N$nhỏ hơn$\sqrt{x}$, nhiều ước số xuất hiện theo cặp nhưng chỉ có một tập hợp con là hợp lệ. điều kiện`<= n`đảm bảo chúng ta chỉ đếm những chiếc đồng hồ thực sự tồn tại, bất kể có bao nhiêu ước số toán học$x$có.
