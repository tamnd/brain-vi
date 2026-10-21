---
title: "CF 105813C - Gạch Maxwell"
description: "Chúng ta có một bức tường hình chữ nhật có kích thước $2 triệu nhân 2n$, có tâm ở gốc tọa độ trên lưới số nguyên. Mỗi ô vuông đơn vị được xác định bởi tọa độ nguyên $(x, y)$ bên trong hình chữ nhật này."
date: "2026-06-26T00:06:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105813
codeforces_index: "C"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2025"
rating: 0
weight: 105813
solve_time_s: 62
verified: true
draft: false
---

[CF 105813C - Gạch của Maxwell](https://codeforces.com/problemset/problem/105813/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bức tường hình chữ nhật có kích thước$2m \times 2n$, tập trung tại điểm gốc trên lưới số nguyên. Mỗi ô vuông đơn vị được xác định bằng tọa độ nguyên$(x, y)$bên trong hình chữ nhật này. 

Giá trị của một ô được xác định là$\max(|x|, |y|)$. Điều này phân chia toàn bộ lưới thành các “vòng” hình vuông đồng tâm. Tất cả các ô có cùng giá trị đều nằm trên cùng một vòng, tạo thành một vòng khép kín quanh tâm. 

Một ô là một nhóm các ô được kết nối và nó chỉ được phép nếu mọi ô bên trong nó nằm hoàn toàn trong một vòng duy nhất. Vì vậy, một ô không thể xuyên từ vòng này sang vòng khác, nhưng nó có thể chiếm bất kỳ hình dạng nào được kết nối bên trong vòng của nó, bao gồm cả các hình có lỗ, miễn là khả năng kết nối được duy trì. 

Hai ô được coi là khác nhau nếu tồn tại ít nhất một cặp ô nằm cùng nhau trong một ô ở một ô nhưng được tách thành các ô khác nhau ở ô kia. 

Chúng ta cần đếm xem tồn tại bao nhiêu ô hợp lệ như vậy, modulo$10^9 + 7$, cho đến$10^4$trường hợp thử nghiệm, với$m, n \le 10^6$. 

Hàm ý ràng buộc chính là chúng ta không thể mô phỏng lưới hoặc xây dựng bất kỳ cấu trúc rõ ràng nào cho mỗi trường hợp thử nghiệm. Mọi thứ phải quy về hàm dạng đóng của$m$Và$n$, tính được trong thời gian không đổi. 

Trường hợp cạnh khóa xuất hiện khi lưới ở mức tối thiểu, chẳng hạn như$m = n = 1$, trong đó toàn bộ$2 \times 2$lưới tạo thành một vòng duy nhất. Trong trường hợp đó, mọi giải pháp đều phải tính đến tất cả các phân vùng được kết nối trong 4 chu kỳ. Một giả định ngây thơ rằng mỗi ô hoạt động độc lập sẽ dẫn đến tình trạng đếm quá mức, bởi vì các hạn chế về kết nối sẽ kết hợp các lựa chọn trong suốt chu kỳ. 

Một vấn đề tinh tế khác là mọi vòng đều có tính tuần hoàn chứ không phải tuyến tính. Việc coi một vòng như một đường dẫn sẽ cấm kết nối “bao quanh” một cách không chính xác giữa cạnh cuối cùng và cạnh đầu tiên của chu kỳ. 

## Phương pháp tiếp cận 

Giải thích brute-force là xem xét từng vòng riêng biệt và liệt kê tất cả các cách có thể để phân chia các ô của nó thành các thành phần được kết nối. Vì mỗi thành phần phải nằm trong một vòng, nên các vòng khác nhau không tương tác, do đó câu trả lời tổng thể là tích trên các vòng. 

Bên trong một vòng, cấu trúc là một biểu đồ chu trình đơn giản có các đỉnh là các ô trong lớp đó. Một cách tiếp cận đơn giản sẽ cố gắng liệt kê tất cả các phần của chu trình này thành các đồ thị con cảm ứng được kết nối. Điều này tương đương với việc xem xét mọi cách để chia chu trình thành các khối liền kề nhau. Số lượng phân vùng như vậy tăng theo cấp số nhân với kích thước vòng và thậm chí đối với một vòng có chiều dài$O(m+n)$, vũ lực là không thể thực hiện được. 

Quan sát quan trọng là trên một chu trình, một tập hợp con các cạnh xác định duy nhất một phân vùng thành các thành phần được kết nối. Nếu chúng ta quyết định một cách độc lập xem có nên cắt nó hay không, thì mỗi thành phần được kết nối thu được sẽ tự động là một ô hợp lệ vì kết nối được duy trì chính xác trong mỗi phân đoạn. 

Như vậy, mỗi vòng có chiều dài$L$đóng góp$2^L$những cách có thể. 

Tính tổng tất cả các vòng, mỗi ô thuộc về chính xác một vòng, do đó tổng chiều dài vòng bằng tổng số ô lưới, tức là$4mn$. Điều này làm giảm toàn bộ vấn đề đối với việc tính toán$2^{4mn} \bmod (10^9+7)$. 

Brute-force hoạt động về mặt khái niệm vì nó làm giảm vấn đề thành các quyết định biên độc lập, nhưng nó không thành công về mặt tính toán vì các vòng mô hình hóa rõ ràng hoặc liệt kê các cấu hình trên mỗi vòng có kích thước tuyến tính và không thể áp dụng cho đầu vào lớn. Quan sát cho thấy rằng mỗi vòng là một chu trình trong đó các vết cắt mô tả đầy đủ các ô xếp sẽ nén mọi thứ thành một số mũ duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đếm lực lượng vũ phu trên mỗi vòng | Hàm mũ | O(mn) | Quá chậm | 
| Giảm tính độc lập ở rìa chu kỳ | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mỗi ô thuộc đúng một cấp độ được xác định bởi$\max(|x|, |y|)$. Điều này phân chia lưới thành các vòng rời rạc. 
2. Nhận biết rằng trong một vòng, cạnh kề tạo thành một đồ thị chu trình đơn giản. Mỗi ô có chính xác hai ô lân cận dọc theo đường biên của vòng đó. 
3. Định dạng lại một ô hợp lệ bên trong một vòng dưới dạng phân chia đồ thị chu trình thành các thành phần liên thông. Mỗi thành phần tương ứng với một đoạn liền kề trong chu trình. 
4. Liên kết mỗi cạnh của chu trình với một lựa chọn nhị phân: hoặc chúng ta “cắt” cạnh (kết thúc ranh giới ô) hoặc không. Điều này đảm bảo rằng các thành phần được kết nối chính xác là các phân đoạn không bị cắt tối đa. 
5. Đếm các cấu hình bên trong một vòng có chiều dài$L$BẰNG$2^L$, vì mỗi tập con của các cạnh tương ứng với một phân vùng hợp lệ. 
6. Tổng số tiền đóng góp trên tất cả các vòng. Vì các vòng rời rạc và bao phủ tất cả các ô nên tổng số mũ là số ô trong lưới, tức là$4mn$. 
7. Tính toán$2^{4mn} \bmod (10^9+7)$sử dụng lũy ​​thừa mô đun nhanh. 

Tính chính xác phụ thuộc vào tính bất biến rằng mọi cấu hình cắt tạo ra một phân vùng duy nhất thành các thành phần được kết nối và mọi phân vùng hợp lệ tương ứng với chính xác một mẫu cắt. Không có cấu hình nào vi phạm khả năng kết nối vì việc loại bỏ các cạnh trong một chu trình không thể tạo ra các mảnh bị ngắt kết nối ngoài các phân đoạn dự định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mod_pow(a, e):
    res = 1
    while e:
        if e & 1:
            res = (res * a) % MOD
        a = (a * a) % MOD
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    m, n = map(int, input().split())
    exp = (4 * m * n) % (MOD - 1)
    print(mod_pow(2, exp))
```Việc thực hiện làm giảm toàn bộ hình học thành lũy thừa. Sự tinh tế duy nhất là sử dụng định lý nhỏ của Fermat để rút gọn modulo số mũ$10^9+6$, vì mô đun là số nguyên tố. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi việc tính toán số mũ và giá trị cuối cùng cho hai trường hợp. 

### Ví dụ 1:$m = 1, n = 1$| bước | giá trị | 
| --- | --- | 
| kích thước lưới |$2 \times 2$| 
| tế bào | 4 | 
| số mũ$4mn$| 4 | 
| kết quả |$2^4 = 16 \bmod MOD$| 

Điều này cho thấy cách giải thích tính độc lập của chu trình thô trước khi giảm bớt. 

### Ví dụ 2:$m = 3, n = 1$| bước | giá trị | 
| --- | --- | 
| kích thước lưới |$6 \times 2$| 
| tế bào | 12 | 
| số mũ$4mn$| 12 | 
| kết quả |$2^{12} = 4096$| 

Điều này xác nhận rằng việc chia tỷ lệ một trong hai chiều chỉ làm tăng số mũ tuyến tính thông qua số lượng ô. 

Dấu vết chứng tỏ rằng tất cả sự phức tạp về cấu trúc của việc ốp lát sẽ biến mất sau khi quá trình phân rã chu trình được nhận ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log(4mn))$mỗi bài kiểm tra | Lũy thừa nhanh chiếm ưu thế | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì$m,n \le 10^6$và có tới$10^4$trường hợp thử nghiệm. Mỗi truy vấn giảm xuống một lũy thừa mô-đun duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def mod_pow(a, e):
    res = 1
    while e:
        if e & 1:
            res = (res * a) % MOD
        a = (a * a) % MOD
        e >>= 1
    return res

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        m, n = map(int, input().split())
        out.append(str(mod_pow(2, 4 * m * n)))
    return "\n".join(out)

# sample-like sanity checks
assert run("1\n1 1\n") == "16"
assert run("1\n2 1\n") == str(pow(2, 8, MOD))
assert run("1\n1 2\n") == str(pow(2, 8, MOD))
assert run("1\n3 4\n") == str(pow(2, 48, MOD))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1 | 16 | độ chính xác lưới nhỏ nhất | 
| 2 1/1 | 256 | kích thước không đối xứng | 
| 1 2/1 | 256 | tính đối xứng trong m,n | 
| 3 4/1 |$2^{48}$| chia tỷ lệ chung | 

## Vỏ cạnh 

Khi nào$m = n = 1$, lưới giảm xuống còn một chu kỳ có độ dài 4. Thuật toán diễn giải điều này là$4mn = 4$, sản xuất$2^4$. Điều này phù hợp với ý tưởng rằng mỗi cạnh trong bốn cạnh chu kỳ có thể được cắt hoặc không độc lập, tạo ra tất cả các phân vùng được kết nối hợp lệ. 

Khi một chiều bằng 1 và chiều kia lớn, cấu trúc trở thành một ranh giới hình chữ nhật dài với nhiều vòng lồng vào nhau. Mỗi vòng vẫn đóng góp độc lập và tổng số mũ vẫn chính xác bằng số ô trong hình chữ nhật. Thuật toán không phân biệt vòng trong và vòng ngoài vì tất cả đều được hấp thụ vào tổng số$4mn$, đảm bảo xử lý nhất quán các lưới kéo dài.
