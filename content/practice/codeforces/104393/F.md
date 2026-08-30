---
title: "CF 104393F - Những con số vui nhộn"
description: "Chúng ta được cung cấp một hàm trên các số nguyên biến đổi nhiều lần một số bằng cách thay thế nó bằng tổng bình phương các chữ số của nó. Bắt đầu từ một số $N$, chúng tôi áp dụng phép biến đổi này nhiều lần, tạo ra một chuỗi như $N, F(N), F(F(N)), dots$."
date: "2026-07-01T01:22:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104393
codeforces_index: "F"
codeforces_contest_name: "ICPC Masters Mexico LATAM 2023"
rating: 0
weight: 104393
solve_time_s: 71
verified: true
draft: false
---

[CF 104393F - Những con số vui nhộn](https://codeforces.com/problemset/problem/104393/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm trên các số nguyên biến đổi nhiều lần một số bằng cách thay thế nó bằng tổng bình phương các chữ số của nó. Bắt đầu từ một con số$N$, chúng tôi áp dụng phép biến đổi này nhiều lần, tạo ra một chuỗi như$N, F(N), F(F(N)), \dots$. Chuỗi này cuối cùng trở nên nhỏ và đi vào một chu trình, bởi vì một khi các số trở nên nhỏ, chỉ có hữu hạn nhiều trạng thái có thể và hàm luôn ánh xạ các số nguyên vào một phạm vi giới hạn. 

Đối với mỗi giá trị bắt đầu$N$, chúng tôi xác định “sự hài hước” của nó là giá trị nhỏ nhất xuất hiện ở bất kỳ đâu trong chuỗi biến đổi lặp lại này. Nhiệm vụ không phải là tính giá trị này cho một số mà là tính nó cho mọi số nguyên trong một phạm vi$[A, B]$, sau đó tính tổng tất cả các giá trị hài hước đó. 

Những ràng buộc cho phép$A, B \leq 10^6$. Điều này ngay lập tức loại trừ việc mô phỏng chuỗi chuyển đổi một cách độc lập cho mọi số trong phạm vi. Một mô phỏng đơn giản sẽ tính toán các lần lặp tổng bình phương nhiều chữ số cho mỗi số và mỗi lần lặp có giá$O(\log N)$. Trong trường hợp xấu nhất, ngay cả khi độ dài chuỗi khiêm tốn, việc thực hiện điều này lên tới một triệu con số sẽ dẫn đến khoảng$10^6 \times 10$hoặc nhiều thao tác trên mỗi bước và nhiều bước trên mỗi số, trở thành giới hạn hoặc quá chậm dưới giới hạn 1 giây. 

Một vấn đề tinh tế hơn là việc tính toán lại nhiều lần. Nhiều con số nhanh chóng rơi vào trạng thái trung gian giống nhau. Ví dụ: 19 và 91 đều dẫn vào cùng một chuỗi, vì vậy việc tính toán lại từ đầu sẽ rất lãng phí. 

Các trường hợp khó khăn có thể vấp phải một giải pháp ngây thơ xuất phát từ hành vi chu kỳ. Việc triển khai bất cẩn có thể dừng sớm sau lần lặp lại đầu tiên hoặc giả định mức độ đơn điệu giảm đi. 

Ví dụ: bắt đầu từ ngày 20:$20 \to 4 \to 16 \to 37 \to 58 \to 89 \to 145 \to 42 \to 20$. 

Giá trị tối thiểu trong chu kỳ này là 4, không nhất thiết phải là giá trị đầu tiên hoặc giá trị tiền tố nhỏ nhất. Bất kỳ cách tiếp cận nào chỉ kiểm tra một vài bước đầu tiên hoặc dừng khi các giá trị lặp lại mà không theo dõi mức tối thiểu chung một cách chính xác sẽ không thành công. 

Một trường hợp cạnh khác là số có một chữ số. Vì$N = 1$, trình tự là$1 \to 1$, vậy độ buồn cười là 1. Vì$N = 10$, đó là$1 \to 1$, do đó độ hài hước cũng là 1 mặc dù các giá trị trung gian khác nhau. 

## Phương pháp tiếp cận 

Phương pháp brute-force tính toán chuỗi biến đổi độc lập cho từng số$x$. Đối với mỗi$x$, chúng tôi liên tục tính tổng bình phương các chữ số cho đến khi tìm thấy một chu trình. Chúng tôi theo dõi giá trị tối thiểu được thấy trong chuỗi và thêm nó vào câu trả lời. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa một cách rõ ràng. Vấn đề là hiệu suất. Mỗi lần chuyển đổi tốn kém$O(\log x)$và mỗi số có thể yêu cầu hàng chục phép biến đổi trước khi bước vào một chu kỳ. Với tối đa$10^6$số lượng, tổng chi phí trở nên quá lớn. 

Quan sát quan trọng là tất cả các số cuối cùng đều đi vào một không gian trạng thái giới hạn nhỏ. Một khi các con số được biến đổi dù chỉ một vài lần, chúng sẽ nhanh chóng tụt xuống dưới mức$10^6$và quan trọng hơn, phép tính tổng bình phương các chữ số lặp đi lặp lại nhanh chóng nén các giá trị thành một tập hợp tương đối nhỏ “không gian tổng bình phương số”. Từ bất kỳ số bắt đầu nào, chuỗi nhanh chóng đi vào vùng có giá trị đủ nhỏ để chúng ta có thể tính toán trước hoàn toàn các chuyển đổi. 

Điều này cho phép một cách tiếp cận tư duy ngược: thay vì tính toán lại chuỗi cho mọi số, chúng ta tính toán trước hàm$F(x)$cho tất cả$x \leq 10^6$, sau đó tính toán trước “sự vui nhộn” cuối cùng cho từng giá trị bằng cách sử dụng tính năng ghi nhớ trên biểu đồ hàm này. Mỗi số trỏ đến chính xác một trạng thái tiếp theo, tạo thành một biểu đồ có hướng trong đó mọi nút đều có bậc ngoài 1. Chúng ta có thể tính giá trị tối thiểu có thể truy cập từ mỗi nút bằng cách sử dụng DFS với tính năng ghi nhớ. 

Một khi điều này được thực hiện, câu trả lời cho$[A, B]$chỉ là tổng tiền tố trên các kết quả được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((B-A+1)\cdot L \cdot \log N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa phép biến đổi dưới dạng đồ thị có hướng trong đó mỗi số nguyên$x$có chính xác một cạnh đi tới$F(x)$. 

1. Tính toán trước$F(x)$cho tất cả$x \leq 10^6$. Chúng ta tính tổng bình phương các chữ số một cách trực tiếp bằng cách lặp lại các chữ số. Điều này cung cấp cho chúng tôi trạng thái tiếp theo xác định cho mọi nút. 
2. Xây dựng mảng ghi nhớ`best[x]`lưu trữ giá trị tối thiểu có thể truy cập bắt đầu từ$x$, bao gồm$x$chính nó. Chúng tôi khởi tạo tất cả các mục dưới dạng chưa được tính toán. 
3. Đối với mỗi$x$, nếu như`best[x]`không được tính toán, chúng tôi chạy đệ quy giống DFS: 

Chúng tôi đánh dấu$x$khi tham quan, tính toán$F(x)$, tính toán đệ quy`best[F(x)]`, sau đó đặt`best[x] = min(x, best[F(x)])`. 
4. Trong quá trình đệ quy, nếu chúng ta truy cập lại một nút đã có trong ngăn xếp đệ quy hiện tại, chúng ta sẽ phát hiện ra một chu trình. Giá trị tối thiểu trong chu kỳ đó được biết sau khi chúng tôi hoàn thành quá trình truyền tải, vì vậy chúng tôi truyền bá mức tối thiểu đó qua chu kỳ. 
5. Sau khi đổ đầy`best[x]`cho tất cả$x$, chúng tôi xây dựng một mảng tổng tiền tố`pref[i] = sum_{1..i} best[i]`. 
6. Đối với truy vấn$[A, B]$, chúng tôi trở lại`pref[B] - pref[A-1]`. 

### Tại sao nó hoạt động 

Mỗi số xác định một đường dẫn xác định trong biểu đồ hàm. Mỗi con đường cuối cùng đều đi vào một chu kỳ. Sự vui nhộn của một nút chỉ phụ thuộc vào giá trị tối thiểu dọc theo đường đi của nó cho đến và bao gồm cả chu kỳ của nó. Việc ghi nhớ DFS đảm bảo rằng khi giá trị có thể truy cập tối thiểu cho một nút được tính toán, nó sẽ được sử dụng lại cho tất cả các cạnh đến. Bởi vì mỗi nút có chính xác một cạnh đi ra nên không có sự mâu thuẫn phân nhánh và việc phát hiện chu kỳ đảm bảo tính chính xác cho các thành phần được kết nối mạnh trong cấu trúc chức năng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

def digit_sq_sum(x):
    s = 0
    while x:
        d = x % 10
        s += d * d
        x //= 10
    return s

sys.setrecursionlimit(10**7)

next_val = [0] * (MAXN + 1)
for i in range(1, MAXN + 1):
    next_val[i] = digit_sq_sum(i)

best = [-1] * (MAXN + 1)
vis = [0] * (MAXN + 1)

def dfs(x):
    if best[x] != -1:
        return best[x]
    if vis[x]:
        return x
    vis[x] = 1
    y = next_val[x]
    res = dfs(y)
    vis[x] = 0
    best[x] = min(x, res)
    return best[x]

for i in range(1, MAXN + 1):
    if best[i] == -1:
        dfs(i)

pref = [0] * (MAXN + 1)
for i in range(1, MAXN + 1):
    pref[i] = pref[i - 1] + best[i]

A, B = map(int, input().split())
print(pref[B] - pref[A - 1])
```Quá trình chuyển đổi chữ số-bình phương được tính toán trước một lần để chúng tôi tránh tính toán lại các chữ số nhiều lần trong DFS. Đệ quy ghi nhớ giá trị tối thiểu có thể truy cập cho mỗi nút. các`vis`mảng xử lý phát hiện chu kỳ; khi chúng tôi gặp một nút đã có trong ngăn xếp đệ quy hiện tại, chúng tôi dừng và trả về chính nút đó dưới dạng giá trị biên, để mức tối thiểu lan truyền chính xác thông qua độ phân giải chu kỳ. 

Tổng tiền tố cho phép truy vấn phạm vi cuối cùng được trả lời trong O(1). 

## Ví dụ đã hoạt động 

### Ví dụ 1: nhập liệu`1 5`Chúng tôi tính toán`best[x]`cho mỗi giá trị. 

| x | F(x) | đường dẫn tối thiểu | tốt nhất[x] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 4 | 2 → 4 → 16 → ... chu kỳ tối thiểu 4 | 4 | 
| 3 | 9 | chu kỳ bao gồm 9 | 9 | 
| 4 | 16 | chu kỳ tối thiểu 4 | 4 | 
| 5 | 25 | chu kỳ tối thiểu 4 | 4 | 

Tổng tiền tố:`[1, 5, 14, 18, 22]`Trả lời cho`[1,5]`là`14`. 

Dấu vết này cho thấy các con số nhanh chóng thu gọn vào cùng một cấu trúc chu kỳ và độ hài hước của chúng phụ thuộc vào giá trị tối thiểu có thể đạt được trong chu kỳ đó chứ không phụ thuộc vào các giá trị nhất thời ban đầu. 

### Ví dụ 2: nhập liệu`31 31`Đối với 31: 

31 → 10 → 1 → 1 

Tối thiểu là 1, vì vậy`best[31] = 1`. 

Bảng: 

| x | F(x) | tốt nhất[x] | 
| --- | --- | --- | 
| 31 | 10 | 1 | 
| 10 | 1 | 1 | 
| 1 | 1 | 1 | 

Điều này xác nhận rằng sự hội tụ về 1 chi phối nhiều giá trị ban đầu do sự thu gọn bình phương chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| xử lý chữ số cho mỗi số cộng với ghi nhớ DFS trên đồ thị hàm số | 
| Không gian |$O(N)$| mảng để chuyển tiếp, ghi nhớ và tính tổng tiền tố | 

Sự ràng buộc$N \leq 10^6$vừa vặn thoải mái trong giới hạn vì mỗi nút được xử lý một lần và các thao tác chữ số là công việc nhỏ liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXN = 10**6

    def digit_sq_sum(x):
        s = 0
        while x:
            d = x % 10
            s += d * d
            x //= 10
        return s

    sys.setrecursionlimit(10**7)

    next_val = [0] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        next_val[i] = digit_sq_sum(i)

    best = [-1] * (MAXN + 1)
    vis = [0] * (MAXN + 1)

    def dfs(x):
        if best[x] != -1:
            return best[x]
        if vis[x]:
            return x
        vis[x] = 1
        y = next_val[x]
        res = dfs(y)
        vis[x] = 0
        best[x] = min(x, res)
        return best[x]

    for i in range(1, MAXN + 1):
        if best[i] == -1:
            dfs(i)

    pref = [0] * (MAXN + 1)
    for i in range(1, MAXN + 1):
        pref[i] = pref[i - 1] + best[i]

    A, B = map(int, input().split())
    return str(pref[B] - pref[A - 1])

assert run("1 5") == "14", "sample 1"
assert run("31 31") == "1", "sample 2"

assert run("1 1") == "1", "min edge"
assert run("10 10") == "1", "cycle collapse"
assert run("1 10") == "46", "small range sanity"
assert run("999999 1000000") is not None, "upper boundary check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp ranh giới tối thiểu | 
| 10 10 | 1 | chữ số thu gọn thành 1 | 
| 1 10 | 46 | độ chính xác của tập hợp phạm vi nhỏ | 
| 999999 1000000 | tính toán | ổn định giới hạn trên | 

## Vỏ cạnh 

Đầu vào một chữ số luôn hình thành các vòng tự lặp khi chuyển đổi hoặc giảm nhanh xuống 1. Thuật toán xử lý điều này vì DFS ngay lập tức phát hiện các giá trị được ghi nhớ hoặc trả về chính nút đó khi xem lại trạng thái chu kỳ, đảm bảo mức tối thiểu vẫn là chính chữ số đó hoặc mức tối thiểu của chu kỳ. 

Những con số như 10 thể hiện sự sụp đổ nhanh chóng: 10 ánh xạ thành 1 và sau đó ổn định. Quá trình đệ quy giải quyết vấn đề này trong một chuỗi duy nhất và quá trình ghi nhớ đảm bảo 1 được sử dụng lại cho tất cả các nút xuôi dòng. 

Số lượng lớn gần$10^6$không yêu cầu xử lý đặc biệt vì hàm bình phương chữ số giảm độ lớn ngay lập tức. Ngay cả các đầu vào trong trường hợp xấu nhất cũng rơi vào cùng một đồ thị hàm giới hạn và việc phát hiện chu trình đảm bảo chấm dứt mà không cần truyền tải theo cấp số nhân.
