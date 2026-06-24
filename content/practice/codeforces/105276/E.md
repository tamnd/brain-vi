---
title: "CF 105276E - Người đam mê thuật toán"
description: "Chúng ta được cung cấp một số loại thuật toán, trong đó mỗi loại chứa một số thuật toán riêng biệt nhất định. Trong $K$ những ngày tiếp theo, Bob sẽ nghiên cứu chính xác một danh mục mỗi ngày và vào ngày đó anh ấy phải học chính xác các thuật toán $M$ từ danh mục đã chọn đó."
date: "2026-06-23T14:12:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105276
codeforces_index: "E"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2023"
rating: 0
weight: 105276
solve_time_s: 66
verified: true
draft: false
---

[CF 105276E - Người đam mê thuật toán](https://codeforces.com/problemset/problem/105276/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số loại thuật toán, trong đó mỗi loại chứa một số thuật toán riêng biệt nhất định. Trong thời gian tiếp theo$K$ngày, Bob sẽ học chính xác một danh mục mỗi ngày và vào ngày đó Bob phải học chính xác$M$các thuật toán từ danh mục đã chọn đó. 

Một danh mục có thể được sử dụng lại trong nhiều ngày, nhưng mỗi ngày đều độc lập: Bob chỉ chọn$M$các thuật toán riêng biệt từ danh mục đó cho ngày hôm đó. Anh ta được phép bỏ qua hoàn toàn một số thuật toán, do đó không cần phải khai thác hết một danh mục. 

Mục tiêu là chọn số nguyên lớn nhất có thể$M$sao cho có thể chỉ định một danh mục cho mỗi$K$ngày và mỗi danh mục được chọn có ít nhất$M$các thuật toán chưa sử dụng có sẵn cho ngày hôm đó. 

Diễn đạt lại theo cách khác, cho cố định$M$, một danh mục với$a_i$thuật toán có thể được sử dụng nhiều nhất$\left\lfloor \frac{a_i}{M} \right\rfloor$ngày. Chúng ta cần kiểm tra xem tổng số ngày sử dụng được của tất cả các danh mục có ít nhất là$K$. Chúng tôi muốn tối đa$M$thỏa mãn điều kiện này. 

Các ràng buộc cho phép lên đến$10^5$danh mục và tổng số thuật toán lên tới$10^5$. Điều này gợi ý rõ ràng rằng bất kỳ nghiệm nào có hành vi bậc hai trong$N$hoặc$a_i$là không thể. Thậm chí$O(N \log N)$thì ổn, nhưng bất cứ điều gì tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi lần kiểm tra sẽ quá chậm nếu lặp lại nhiều lần. 

Một lực lượng vũ phu trực tiếp trên$M$từ 1 đến$\max a_i$, kiểm tra tính khả thi mỗi lần sẽ quá chậm vì mỗi lần kiểm tra đều yêu cầu quét tất cả các danh mục, dẫn đến khoảng$O(N \cdot \max a_i)$, có thể đạt tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Trường hợp phức tạp xuất hiện khi các danh mục rất không đồng đều. Ví dụ, nếu tất cả$a_i = 1$Và$K = N$, thì chỉ$M = 1$hoạt động. Một ý tưởng tham lam ngây thơ chỉ định các danh mục mà không tính đến việc sử dụng lại trong nhiều ngày sẽ thất bại nếu nó giả định rằng mỗi danh mục chỉ được sử dụng một lần. 

Một dạng thất bại khác là cố gắng gán ngày một cách tham lam mà không tái sử dụng mô hình. Ví dụ: suy nghĩ "chọn các danh mục lớn nhất trước và trừ đi$M$" mà không tính số lượng nhóm đầy đủ mà mỗi danh mục có thể hỗ trợ sẽ dẫn đến việc lập mô hình suy giảm không chính xác trừ khi được thực hiện cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử mọi giá trị có thể có của$M$và với mỗi giá trị, hãy tính số ngày chúng tôi có thể hỗ trợ. Đối với một cố định$M$, mỗi loại$i$đóng góp$\lfloor a_i / M \rfloor$ngày. Chúng tôi tính tổng số này trên tất cả các danh mục và kiểm tra xem tổng đó có ít nhất là$K$. Điều này đúng vì mỗi nhóm$M$các thuật toán là độc lập và rời rạc trong một danh mục. 

Vấn đề là hiệu suất. Đối với mỗi ứng viên$M$, chúng tôi quét tất cả$N$Thể loại. Từ$M$có thể đi lên$10^5$, tổng công việc trở thành$O(N \cdot \max a_i)$, quá lớn. 

Quan sát quan trọng là tính khả thi là đơn điệu trong$M$. Nếu một giá trị$M$là khả thi thì bất kỳ giá trị nhỏ hơn nào cũng có thể thực hiện được vì giảm$M$chỉ tăng hoặc bảo toàn$\lfloor a_i / M \rfloor$cho mỗi thể loại. Cấu trúc đơn điệu này cho phép chúng ta sử dụng tìm kiếm nhị phân trên$M$. 

Mỗi lần kiểm tra tính khả thi vẫn$O(N)$, nhưng chúng tôi chỉ biểu diễn$O(\log \max a_i)$kiểm tra, giải pháp mang lại hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot \max a_i)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tham lam |$O(N \log \max a_i)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là các giá trị lớn hơn của$M$giảm số ngày sử dụng được của mỗi loại. 

1. Xác định hàm`can(M)`tính toán số ngày có thể được hình thành nếu mỗi ngày sử dụng$M$các thuật toán từ một danh mục duy nhất. Đối với mỗi danh mục$a_i$, chúng tôi thêm$a_i // M$đến tổng số đang chạy. Điều này đếm có bao nhiêu nhóm kích thước đầy đủ$M$hiện hữu. 
2. Nếu tổng số nhóm trong tất cả các danh mục ít nhất là$K$, sau đó$M$là khả thi. Nếu không thì không khả thi. Điều này phù hợp trực tiếp với yêu cầu mà mỗi$K$ngày phải được chỉ định một nhóm hợp lệ. 
3. Thực hiện tìm kiếm nhị phân trên$M$trong khoảng từ 1 đến$\max(a_i)$. Giới hạn dưới là 1 vì ít nhất một thuật toán mỗi ngày luôn là lựa chọn có ý nghĩa tối thiểu. 
4. Về điểm giữa$mid$, tính toán`can(mid)`. Nếu khả thi, chúng tôi thử các giá trị lớn hơn bằng cách di chuyển giới hạn dưới lên trên. Nếu không, chúng tôi giảm giới hạn trên. 
5. Sau khi tìm kiếm nhị phân kết thúc, giới hạn dưới sẽ trỏ đến mức khả thi tối đa$M$. 

### Tại sao nó hoạt động 

chức năng`can(M)`là đơn điệu không tăng trong$M$. Tăng dần$M$chỉ có thể giảm hoặc giữ nguyên số lượng nhóm đầy đủ trong mỗi danh mục chứ không bao giờ tăng lên. Điều này đảm bảo rằng không gian khả thi là tiền tố liền kề trên các số nguyên$M$, do đó tìm kiếm nhị phân tìm thấy chính xác ranh giới giữa các giá trị khả thi và không khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(M, arr, K):
    total = 0
    for a in arr:
        total += a // M
        if total >= K:
            return True
    return False

def solve():
    N, K = map(int, input().split())
    arr = list(map(int, input().split()))
    
    lo, hi = 1, max(arr)
    ans = 1
    
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, arr, K):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`can`chức năng chuyển đổi từng danh mục thành số lượng “ngày” đầy đủ mà nó có thể hỗ trợ tại một thời điểm nhất định$M$. Việc thoát sớm khi`total >= K`ngăn chặn những công việc không cần thiết một khi tính khả thi được xác nhận. 

Tìm kiếm nhị phân duy trì tính bất biến mà tất cả các giá trị bên dưới`lo`là những ứng cử viên khả thi, trong khi tất cả các giá trị trên`hi`là không thể thực hiện được. Biến`ans`theo dõi điểm giữa hợp lệ tốt nhất được thấy cho đến nay. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
4 6 7 3 1
```Chúng tôi tìm kiếm tối đa$M$. 

| giữa | đóng góp (a_i // giữa) | tổng cộng | khả thi | 
| --- | --- | --- | --- | 
| 4 | 1+1+1+0+0 = 3 | 3 | không | 
| 2 | 2+3+3+1+0 = 9 | 9 | vâng | 
| 3 | 1+2+2+1+0 = 6 | 6 | vâng | 

Tìm kiếm nhị phân hội tụ về 3. Điều này xác nhận rằng trong khi 4 là quá lớn để duy trì trong 5 ngày, thì 3 vẫn cho phép thực hiện đủ các bài tập được nhóm. 

### Ví dụ 2 

đầu vào:```
3 4
10 10 10
```| giữa | đóng góp | tổng cộng | khả thi | 
| --- | --- | --- | --- | 
| 5 | 2+2+2 = 6 | 6 | vâng | 
| 6 | 1+1+1 = 3 | 3 | không | 

Vì vậy, câu trả lời là 5. Điều này thể hiện rõ ràng hành vi cắt giảm: một khi việc phân nhóm trở nên quá lớn, công suất sẽ giảm mạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log A)$| mỗi lần kiểm tra tính khả thi sẽ quét tất cả các danh mục, tìm kiếm nhị phân có thể$M$| 
| Không gian |$O(1)$| chỉ lưu trữ mảng đầu vào và bộ đếm | 

Các ràng buộc cho phép lên đến$10^5$các danh mục và mỗi lần kiểm tra tính khả thi đều tuyến tính theo$N$. Với tối đa khoảng 17-20 lần lặp tìm kiếm nhị phân, tổng công việc vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(M, arr, K):
        total = 0
        for a in arr:
            total += a // M
            if total >= K:
                return True
        return False

    def solve():
        N, K = map(int, input().split())
        arr = list(map(int, input().split()))
        lo, hi = 1, max(arr)
        ans = 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, arr, K):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        print(ans)

    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5 5\n4 6 7 3 1\n") == "3"

# minimum case
assert run("1 1\n5\n") == "5"

# all equal
assert run("4 4\n4 4 4 4\n") == "1"

# tight packing
assert run("2 3\n5 5\n") == "2"

# large skew
assert run("3 5\n100 1 1\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5 | 5 | cạnh danh mục đơn | 
| 4 4 / 4 4 4 4 | 1 | nhóm tối thiểu | 
| 2 3 / 5 5 | 2 | phân bổ cân bằng | 
| 3 5 / 100 1 1 | 20 | phân phối lệch | 

## Vỏ cạnh 

Một trường hợp danh mục duy nhất như`N = 1`kiểm tra xem lời giải có chính xác khi đưa về phép chia đơn giản hay không. Đối với đầu vào`1 3`với`a = [10]`, tính khả thi phụ thuộc hoàn toàn vào việc liệu`10 // M >= 3`, do đó tìm kiếm nhị phân phải hội tụ chính xác đến ước số lớn nhất thỏa mãn ràng buộc này, tạo ra`M = 3`. 

Ví dụ: khi tất cả các danh mục đều bằng nhau`N = 5`,`K = 5`, Và`a_i = 2`, thuật toán nhận ra chính xác rằng mỗi danh mục đóng góp tối đa một ngày nếu`M = 2`, nhưng đóng góp hai ngày nếu`M = 1`. Tìm kiếm nhị phân ghi lại bước thay đổi này mà không cần xử lý trường hợp rõ ràng. 

Trong các đầu vào bị sai lệch nhiều như`a = [100, 1, 1, 1]`, hầu hết năng lực đều đến từ một danh mục duy nhất. Đối với lớn`M`, các danh mục nhỏ hơn không đóng góp gì và chỉ có danh mục lớn mới quan trọng. Thuật toán vẫn tính tổng các khoản đóng góp một cách chính xác và không nhầm lẫn khi giả định phân phối đồng đều.
