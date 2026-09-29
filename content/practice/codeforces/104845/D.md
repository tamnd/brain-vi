---
title: "CF 104845D - \u041c\u0435\u0433\u0430 HOẶC"
description: "Chúng tôi duy trì một mảng rất lớn được lập chỉ mục lên tới $10^9$, nhưng chỉ có các vị trí $n$ đầu tiên ban đầu là khác 0. Tất cả các vị trí còn lại hoàn toàn bằng 0. Mỗi vị trí chứa một số nguyên 30 bit. Có hai hoạt động. Đầu tiên, chúng ta có thể cập nhật một vị trí thành một giá trị mới."
date: "2026-06-28T11:30:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104845
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104845
solve_time_s: 94
verified: false
draft: false
---

[CF 104845D - \u041c\u0435\u0433\u0430 OR](https://codeforces.com/problemset/problem/104845/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng rất lớn được lập chỉ mục lên đến$10^9$, nhưng chỉ là lần đầu tiên$n$các vị trí ban đầu khác 0. Tất cả các vị trí còn lại hoàn toàn bằng 0. Mỗi vị trí chứa một số nguyên 30 bit. 

Có hai hoạt động. Đầu tiên, chúng ta có thể cập nhật một vị trí thành một giá trị mới. Thứ hai, chúng ta được yêu cầu đếm có bao nhiêu số nguyên không âm$x$thỏa mãn ràng buộc bitwise toàn cục: nếu chúng ta lấy OR theo bit của mọi phần tử mảng cùng với$x$, kết quả không vượt quá một giới hạn nhất định$z$. 

Quan sát quan trọng là OR trên tất cả các phần tử mảng sẽ thu gọn toàn bộ cấu trúc thành một giá trị duy nhất. Nếu chúng ta định nghĩa$$S = a_1 \,|\, a_2 \,|\, \cdots \,|\, a_n,$$thì mọi phần tử 0 bổ sung không đóng góp gì, vì vậy OR toàn cục chính xác là$S$. Sau khi thêm$x$, điều kiện trở thành$$S \,|\, x \le z.$$Các ràng buộc rất lớn: lên tới$10^5$cập nhật và truy vấn cũng như các giá trị nằm trong phạm vi 30 bit. Bất kỳ giải pháp nào tính toán lại OR từ đầu cho mỗi truy vấn đều quá chậm nếu nó chạm vào tất cả các phần tử, vì điều đó sẽ tốn kém$O(nq)$, đó là về$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một điểm tinh tế là việc cập nhật có thể xảy ra ở các chỉ số tùy ý lên tới$10^9$, nhưng chỉ những chỉ số được cung cấp ban đầu mới quan trọng. Bất kỳ vị trí nào khác vĩnh viễn bằng 0, do đó các cập nhật bên ngoài tập hợp ban đầu có thể bị bỏ qua về mặt khái niệm trừ khi chúng đề cập đến tập hợp ban đầu.$n$hoặc mở rộng tập hoạt động theo cách hiểu tổng quát hơn. 

Các trường hợp cạnh phát sinh khi các bit bị hủy hoặc xuất hiện lại: 

Nếu tất cả các giá trị trở thành 0 thì điều kiện giảm xuống$x \le z$, cho$z+1$giải pháp. Nếu như$S$đã vượt quá$z$, không có giá trị của$x$có thể sửa các bit cao hơn trong$S$, vì vậy câu trả lời là không. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp duy trì toàn bộ mảng và tính toán lại OR cho mọi truy vấn loại 2. Điều này đúng vì OR có tính kết hợp và giao hoán, do đó việc tính toán lại từ đầu luôn mang lại kết quả đúng$S$. Tuy nhiên, mỗi truy vấn sẽ yêu cầu quét tối đa$n$các yếu tố, dẫn đến$O(nq)$công việc. Với$n,q \approx 10^5$, điều này trở nên quá chậm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là OR hoạt động đơn điệu trên mỗi bit. Mỗi bit hiện diện hoặc vắng mặt ở trạng thái chung và các bản cập nhật chỉ chuyển đổi các bit trong một tập hợp cố định. Chúng tôi không cần biết các phần tử riêng lẻ cho loại truy vấn 2, chỉ có OR toàn cục hiện tại$S$. 

Vì vậy, chúng tôi duy trì một mặt nạ bit duy nhất$S$. Để cập nhật, chúng tôi phải điều chỉnh$S$khi một phần tử thay đổi. Nếu một giá trị được thay thế, một số bit có thể biến mất khỏi OR nếu giá trị đó là thành phần đóng góp cuối cùng của các bit đó. Điều này yêu cầu theo dõi, trên mỗi bit, có bao nhiêu phần tử hiện chứa nó. 

Chúng tôi duy trì dải tần số có kích thước 30 để đếm số bit. Khi cập nhật một phần tử, chúng ta giảm số lượng cho giá trị cũ và tăng cho giá trị mới của nó. OR toàn cục được cập nhật bằng cách đặt một bit nếu số đếm của nó trở thành dương và xóa nó nếu nó trở thành 0. 

Một lần$S$đã biết, mỗi truy vấn giảm xuống còn việc đếm$x$như vậy$$S \,|\, x \le z.$$Đây là một vấn đề đếm ràng buộc bit cổ điển. Đối với bất kỳ bit nào ở đâu$S$có số 1 thì kết quả đã có số 1 bất kể$x$. Nếu như$z$có số 0 ở vị trí đó thì không thể được. Nếu không, những bit đó bị ép buộc. Đối với các bit ở đó$S$có 0,$x$phải nằm trong giới hạn do$z$, và số lượng giảm xuống chỉ còn tự do lựa chọn ở những vị trí mà$z$có 1 và$S$có 0. Số hợp lệ$x$do đó là:$$2^{\text{count of bits } i \text{ where } S_i = 0 \text{ and } z_i = 1}$$nếu tất cả các bit ở đâu$S_i = 1$thỏa mãn$z_i = 1$, nếu không thì bằng không. 

Điều này làm giảm mỗi truy vấn xuống$O(30)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tính toán lại HOẶC từng truy vấn |$O(nq)$|$O(n)$| Quá chậm | 
| Duy trì số lượng bit + đếm bit |$O((n+q)\cdot 30)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mảng hoạt động chỉ là mảng đầu tiên$n$các phần tử, vì các vị trí khác bằng 0 và không liên quan. 

1. Đọc mảng ban đầu và tính mảng tần số 30 bit`cnt`Ở đâu`cnt[b]`hiện tại có bao nhiêu số có bit$b$bộ. Điều này cho phép chúng tôi xây dựng lại OR toàn cục mà không cần quét mảng. 
2. Xây dựng mặt nạ OR ban đầu$S$bằng cách thiết lập bit$b$nếu như`cnt[b] > 0`. Điều này nén toàn bộ trạng thái mảng thành một số nguyên duy nhất. 
3. Đối với mỗi truy vấn cập nhật$1\ i\ v$, lấy giá trị cũ tại vị trí$i$. Đối với mỗi bit$b$, nếu đặt ở giá trị cũ thì giảm`cnt[b]`. Sau đó, đối với giá trị mới, tăng các bit tương ứng. Sau khi điều chỉnh số lượng, chỉ tính toán lại các bit bị ảnh hưởng của$S$: chút$b$được thiết lập trong$S$nếu như`cnt[b] > 0`. Điều này đảm bảo$S$luôn phản ánh OR đúng của tất cả các phần tử. 
4. Đối với mỗi truy vấn$2\ z$, trước tiên hãy kiểm tra tính khả thi: nếu$(S \& \sim z) \ne 0$, thì một số bit được yêu cầu bởi$S$nhưng bị cấm bởi$z$, vì vậy câu trả lời là không. 
5. Ngược lại, lặp lại tất cả các bit trong đó$S$có số không. Đối với mỗi bit như vậy$b$, nếu như$z$có chút$b$, nó miễn phí và đóng góp một bậc tự do. Câu trả lời là$2^{k}$, Ở đâu$k$là số bit trống như vậy. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến:`cnt[b] > 0`nếu và chỉ nếu bit$b$hiện diện trong ít nhất một phần tử mảng đang hoạt động. Vì vậy mặt nạ được duy trì$S$luôn chính xác theo bit HOẶC của tất cả các phần tử hiện tại. 

Một lần$S$được cố định, mỗi vị trí bit trong$x$đóng góp một cách độc lập. Một chút thiết lập$S$buộc cùng một bit trong$z$là 1; nếu không thì không có giải pháp nào tồn tại. Bit ở đâu$S$là 0 không áp đặt giới hạn dưới và chỉ bị ràng buộc bởi$z$. Vì mỗi bit như vậy có thể được chọn tự do trong$x$, sự độc lập của các bit mang lại số lũy thừa thuần túy bằng hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    MAXB = 30
    cnt = [0] * MAXB
    
    for v in a:
        for b in range(MAXB):
            if v >> b & 1:
                cnt[b] += 1
    
    S = 0
    for b in range(MAXB):
        if cnt[b] > 0:
            S |= (1 << b)
    
    for i in range(n):
        pass  # placeholder; a is used directly
    
    for _ in range(int(input())):
        tmp = input().split()
        t = int(tmp[0])
        
        if t == 1:
            i = int(tmp[1]) - 1
            v = int(tmp[2])
            old = a[i]
            
            for b in range(MAXB):
                if old >> b & 1:
                    cnt[b] -= 1
                if v >> b & 1:
                    cnt[b] += 1
            
            a[i] = v
            
            S = 0
            for b in range(MAXB):
                if cnt[b] > 0:
                    S |= (1 << b)
        
        else:
            z = int(tmp[1])
            
            if S & ~z:
                print(0)
                continue
            
            free_bits = 0
            for b in range(MAXB):
                if not (S >> b & 1) and (z >> b & 1):
                    free_bits += 1
            
            print(1 << free_bits)

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi tách biệt việc duy trì trạng thái khỏi việc đánh giá truy vấn. Mảng`cnt`theo dõi sự hiện diện trên mỗi bit, trong khi`S`được tính toán lại từ nó sau mỗi lần cập nhật. Điều này tránh mọi nhu cầu duy trì cấu trúc phân đoạn phức tạp vì hoạt động hoàn toàn dựa trên OR. 

Việc kiểm tra tính khả thi`S & ~z`là dạng thu gọn của việc phát hiện các bit bị cấm. Việc liệt kê các bit là const
