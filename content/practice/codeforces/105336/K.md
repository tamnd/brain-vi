---
title: "CF 105336K - \u53d6\u6c99\u5b50\u6e38\u620f"
description: "Có một đống đá $n$ và hai người chơi luân phiên nhau, Alice đi trước. Ở mỗi lần di chuyển, người chơi sẽ loại bỏ một số lượng đá dương khỏi đống. Kích thước của một bước di chuyển bị hạn chế theo hai cách."
date: "2026-06-23T15:26:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105336
codeforces_index: "K"
codeforces_contest_name: "The 2024 CCPC Online Contest"
rating: 0
weight: 105336
solve_time_s: 66
verified: true
draft: false
---

[CF 105336K - \u53d6\u6c99\u5b50\u6e38\u620f](https://codeforces.com/problemset/problem/105336/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có một đống$n$đá và hai người chơi thay phiên nhau, Alice đi trước. Ở mỗi lần di chuyển, người chơi sẽ loại bỏ một số lượng đá dương khỏi đống. Kích thước của một bước di chuyển bị hạn chế theo hai cách. Nó không thể vượt quá$k$, và nó phải tương thích với tất cả các nước đi trước đó theo nghĩa là nó phải phân chia mọi nước đi đã chọn trước đó. Ở nước đi đầu tiên không có giới hạn về số chia hết nên Alice chỉ cần chọn một số giữa$1$Và$k$và không vượt quá số đá còn lại. 

Trò chơi kết thúc khi người chơi loại bỏ được những viên đá cuối cùng và người chơi đó thắng. Cả hai người chơi đều chơi tối ưu. 

Đầu vào cung cấp nhiều trò chơi độc lập. Mỗi trường hợp thử nghiệm chỉ định$n$Và$k$, và chúng ta phải xác định liệu Alice có bị buộc phải thắng hay không. 

Các ràng buộc đạt tới$10^9$cho cả hai$n$Và$k$, với tối đa$10^4$các trường hợp thử nghiệm, vì vậy mọi giải pháp đều phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc mô phỏng cây trò chơi hoặc lặp lại các bước đi có thể xảy ra đều không thể thực hiện được ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 1$. Trong tình huống đó, mọi nước đi buộc phải chính xác bằng 1 bất cứ khi nào hợp pháp và điều kiện chia hết trở nên không liên quan nhưng vẫn hợp lệ. Một điểm tế nhị khác là sau nước đi đầu tiên, ràng buộc chia hết bắt đầu hạn chế rất nhiều nước đi, nhưng nó không loại bỏ nước đi “1” bất cứ khi nào$k \ge 1$, điều này thay đổi hoàn toàn cấu trúc của trò chơi còn lại. 

Ví dụ, nếu$n = 5, k = 1$, Alice buộc phải lấy 1 viên, và trò chơi trở thành việc loại bỏ 1 viên đá luân phiên đơn giản. Nếu như$n = 4, k = 3$, Alice có nhiều bước đi đầu tiên, nhưng sau đó cấu trúc sụp đổ thành một quy trình bị hạn chế cao độ. Một giả định ngây thơ rằng trò chơi chỉ phụ thuộc vào động lực của gcd mà không nhận thấy sự sẵn có liên tục của nước đi 1 sẽ dẫn đến kết luận sai. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp cố gắng mô hình hóa trạng thái trò chơi dưới dạng các viên đá còn lại cùng với gcd của tất cả các nước đi trước đó. Mỗi lượt liệt kê tất cả các ước số hợp lệ của gcd đó không vượt quá$k$, và không lớn hơn đống còn lại. Điều này nắm bắt chính xác các quy tắc, nhưng về nguyên tắc, hệ số phân nhánh vẫn quá lớn và cây trò chơi phát triển nhanh chóng mặc dù$n$là lớn. Vấn đề chính không chỉ là hiệu suất mà còn là việc lý luận về cách chơi tối ưu trên các trạng thái gcd khác nhau cũng trở nên phức tạp. 

Sự đơn giản hóa chính xuất phát từ việc quan sát thấy rằng sau bước đi đầu tiên, hạn chế về khả năng chia hết không làm giảm đáng kể các lựa chọn trong hầu hết các trường hợp, vì số 1 luôn có giá trị. Vì 1 chia hết mọi số nguyên và luôn nằm trong giới hạn$k$, trò chơi sau nước đi đầu tiên sẽ chuyển sang một quá trình trừ bắt buộc trong đó cả hai người chơi luôn có thể lấy 1 cho đến khi hết cọc. 

Điều này biến toàn bộ trò chơi thành một lựa chọn nước đi đầu tiên, sau đó là một trò chơi chẵn lẻ xác định. Quyết định của Alice quy về việc chọn nước đi đầu tiên$a_1$, sau đó trò chơi còn lại là trò chơi đấu một thuần túy$n - a_1$đá bắt đầu từ Bob. 

Brute Force thử tất cả các bước đi đầu tiên và sau đó mô phỏng trò chơi còn lại. Giải pháp tối ưu hóa nén giai đoạn thứ hai thành kiểm tra tính chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của các trạng thái trò chơi | Hàm mũ | O(1) | Quá chậm | 
| Bước đầu tiên + giảm tính chẵn lẻ | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xử lý nước đi thắng tầm thường 

Nếu$n \le k$, Alice có thể lấy tất cả quân trong nước đi đầu tiên của mình vì ban đầu không có hạn chế về khả năng chia. Cô ấy thắng ngay lập tức. 

### 2. Giảm game sau nước đi đầu tiên 

Giả sử Alice lấy$a_1$, Ở đâu$1 \le a_1 \le k$Và$a_1 \le n$. Sau nước đi này, cọc còn lại là$r = n - a_1$, và mọi chuyển động trong tương lai đều phải chia$a_1$. 

Quan sát quan trọng là giá trị 1 luôn là một nước đi hợp lệ vì nó chia hết mọi số nguyên và luôn thỏa mãn$1 \le k$. 

### 3. Thu gọn trò chơi thành quy trình chơi một lần 

Kể từ thời điểm này, cả hai người chơi luôn có thể lấy đúng 1 viên đá. Bất kỳ nước đi nào được phép khác đều không ảnh hưởng đến lối chơi tối ưu vì chúng không bao giờ cần thiết để duy trì tính hợp pháp và luôn có sẵn nước đi 1. 

Do đó, phần còn lại của trò chơi trở thành một quá trình xen kẽ đơn giản, trong đó mỗi người chơi loại bỏ 1 viên đá. 

### 4. Xác định người thắng ván còn lại 

Trong trò chơi đấu một, người chơi đối mặt với số lượng quân lẻ luôn giành chiến thắng với lối chơi tối ưu. Bob đi trước trong ván còn lại nên Bob thắng nếu$r$thật kỳ quặc. 

Alice muốn Bob đối mặt với số quân chẵn nên cô ấy muốn$$n - a_1 \equiv 0 \pmod 2$$tương đương với việc chọn$a_1$có cùng độ chẵn như$n$. 

### 5. Kiểm tra tính khả thi của việc lựa chọn nước đi đó 

Alice có thể chọn bất kỳ$a_1 \le k$. Vì vậy: 

- Nếu$n$thật kỳ quặc, cô ấy cần một điều kỳ quặc$a_1$, và luôn có sẵn 1 nên cô ấy thắng. 
- Nếu như$n$là chẵn, cô ấy cần một chẵn$a_1$. Điều này chỉ có thể thực hiện được nếu$k \ge 2$, vì nước đi chẵn nhỏ nhất là 2. 

### Tại sao nó hoạt động 

Điều bất biến là sau nước đi đầu tiên, tập hợp nước đi hợp lệ luôn gồm 1, và 1 là đủ để mô phỏng cách chơi tối ưu ở ván còn lại. Phần còn lại của nước đi không thể cải thiện kết quả của một trong hai người chơi vì bất kỳ sai lệch nào so với việc lấy liên tục 1 đều không làm thay đổi tính tất yếu dựa trên tính chẵn lẻ của trạng thái cuối. Do đó, trò chơi chuyển sang kiểm soát tính chẵn lẻ của cọc còn lại sau nước đi đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())

        if n <= k:
            out.append("Alice")
            continue

        if n % 2 == 1:
            out.append("Alice")
        else:
            if k >= 2:
                out.append("Alice")
            else:
                out.append("Bob")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp phản ánh việc giảm thiểu. Điều kiện đầu tiên xử lý việc chấm dứt ngay lập tức khi Alice có thể lấy mọi thứ. Nhánh thứ hai mã hóa điều kiện chẵn lẻ bắt nguồn từ giai đoạn bắt buộc “lấy 1”. Không cần mô phỏng các ước số hoặc trạng thái gcd, vì nước đi 1 chi phối tất cả các khả năng trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5, k = 3$Alice không thể kết thúc trò chơi ngay lập tức nên cô ấy chọn nước đi đầu tiên. Quyết định tối ưu dựa trên tính chẵn lẻ. 

| Bước | Người chơi | Di chuyển | Còn lại$n$| 
| --- | --- | --- | --- | 
| 1 | Alice | 1 | 4 | 
| 2 | Bob | 1 | 3 | 
| 3 | Alice | 1 | 2 | 
| 4 | Bob | 1 | 1 | 
| 5 | Alice | 1 | 0 | 

Alice thắng vì cô ấy chọn nước đi để lại số quân chẵn và Bob buộc phải thua tính chẵn lẻ. 

Điều này chứng tỏ rằng một khi nước đi đầu tiên được ấn định, phần còn lại của trò chơi là một chuỗi luân phiên bắt buộc. 

### Ví dụ 2:$n = 6, k = 1$Alice không có lựa chọn nào cho bước đi đầu tiên. 

| Bước | Người chơi | Di chuyển | Còn lại$n$| 
| --- | --- | --- | --- | 
| 1 | Alice | 1 | 5 | 
| 2 | Bob | 1 | 4 | 
| 3 | Alice | 1 | 3 | 
| 4 | Bob | 1 | 2 | 
| 5 | Alice | 1 | 1 | 
| 6 | Bob | 1 | 0 | 

Bob thắng vì cọc còn lại sau nước đi bắt buộc của Alice là số lẻ. 

Điều này cho thấy tại sao ngay cả$n$trở nên thua cuộc khi$k = 1$, vì Alice không thể điều chỉnh tính chẵn lẻ ở nước đi đầu tiên của mình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm được giải quyết bằng một số lần kiểm tra số học không đổi | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Lời giải dễ dàng nằm trong giới hạn vì thậm chí$10^4$trường hợp chỉ yêu cầu các phép toán số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        if n <= k:
            res.append("Alice")
        elif n % 2 == 1:
            res.append("Alice")
        elif k >= 2:
            res.append("Alice")
        else:
            res.append("Bob")
    return "\n".join(res)

# provided samples
assert run("3\n2 3\n4 3\n5 1\n") == "Alice\nAlice\nAlice"

# custom cases
assert run("1\n1 1\n") == "Alice", "minimum win"
assert run("1\n2 1\n") == "Bob", "even n, k=1 loss"
assert run("1\n10 1\n") == "Bob", "forced parity loss"
assert run("1\n10 5\n") == "Alice", "k>=2 allows parity fix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | Alice | thắng ngay khi n ≤ k | 
| 2 1 | Bob | tổn thất bắt buộc theo k = 1, chẵn n | 
| 10 1 | Bob | chuỗi chẵn lẻ thua thậm chí n | 
| 10 5 | Alice | khả năng chọn nước đi đầu tiên | 

## Vỏ cạnh 

###Trường hợp:$n \le k$đầu vào:```
n = 7, k = 10
```Alice ngay lập tức lấy hết 7 viên đá và giành chiến thắng. Thuật toán nắm bắt điều này trực tiếp trong điều kiện đầu tiên, ngăn chặn mọi lý do không cần thiết về tính chẵn lẻ hoặc các động thái trong tương lai. 

### Trường hợp:$k = 1$đầu vào:```
n = 6, k = 1
```Alice buộc phải lấy 1 viên, để lại 5 viên đá. Trò chơi còn lại là sự thay đổi nghiêm ngặt của việc lấy 1. Thuật toán phân loại đây là vị trí thua của Alice vì$n$chẵn và cô ấy không thể điều chỉnh tính chẵn lẻ. 

### Vỏ: lớn$k$, thậm chí$n$đầu vào:```
n = 10, k = 100
```Alice chọn nước đi chẵn đầu tiên chẳng hạn như 2, để lại 8 quân. Phần còn lại trở thành số chẵn cho Bob, vì vậy Alice thắng. điều kiện$k \ge 2$nắm bắt chính xác tính linh hoạt này. 

### Vỏ: lớn$n$, bé nhỏ$k$đầu vào:```
n = 999999999, k = 1
```Alice phải lấy 1, và số chẵn lẻ của cọc còn lại sẽ quyết định người chiến thắng. Từ$n$thật kỳ lạ, mặc dù Alice vẫn thắng$k$là tối thiểu, cho thấy tính chẵn lẻ chiếm ưu thế khi quyền tự do di chuyển biến mất.
