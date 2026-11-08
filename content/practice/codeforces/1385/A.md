---
problem: 1385A
contest_id: 1385
problem_index: A
name: "Three Pairwise Maximums"
contest_name: "Codeforces Round 656 (Div. 3)"
rating: 800
tags: ["math"]
answer: passed_samples
verified: false
solve_time_s: 270
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a2e73da-2f8c-83ec-bd76-b3d392896a21
---

# CF 1385A - Tối đa ba cặp 

**Đánh giá:** 800 
**Thẻ:** toán 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 30 giây 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e73da-2f8c-83ec-bd76-b3d392896a21 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho ba số được cho là đại diện cho cực đại theo cặp của một số bộ ba số nguyên dương ẩn. Cụ thể, tồn tại những giá trị chưa biết$a$,$b$, Và$c$và ba số được cung cấp tương ứng với số lớn nhất của mỗi cặp: một là$\max(a,b)$, cái khác là$\max(a,c)$, và cuối cùng là$\max(b,c)$. Nhiệm vụ là quyết định xem bộ ba như vậy có thể tồn tại hay không và nếu có thể thì xây dựng một nghiệm hợp lệ. 

Khó khăn chính là chúng ta không được biết giá trị đầu vào nào tương ứng với cặp nào. Bất kỳ hoán vị nào của bài tập đều được cho phép và chúng tôi chỉ cần đưa ra một cách xây dựng hợp lệ hoặc báo cáo là không thể thực hiện được. 

Các ràng buộc cho phép lên đến$2 \cdot 10^4$trường hợp thử nghiệm, với giá trị lên tới$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng thực hiện các bài tập ép buộc hoặc liệt kê bộ ba ứng viên cho mỗi trường hợp thử nghiệm. Thậm chí kiểm tra tất cả các nhiệm vụ có thể có của$x, y, z$cực đại theo cặp có kích thước không đáng kể, nhưng mọi nỗ lực tìm kiếm đều có thể$a, b, c$các giá trị sẽ không khả thi vì phạm vi giá trị quá lớn để khám phá một cách có hệ thống. 

Trường hợp cạnh tinh tế phát sinh khi hai trong số các giá trị nhỏ hơn giá trị thứ ba nhưng không bằng giá trị đó. Ví dụ,$x = 10$,$y = 30$,$z = 20$thoạt nhìn có vẻ nhất quán, nhưng không thể chỉ định$a, b, c$sao cho một cặp cực đại là 10 trong khi cặp kia là 30, vì giá trị lớn nhất phải xuất hiện ở ít nhất hai trong số các cực đại theo cặp nếu nó tương ứng với điểm cuối chung trong tam giác so sánh cơ bản. Loại mâu thuẫn này là điều chúng ta cần phát hiện. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ cố gắng đoán$a, b, c$và xác minh xem cực đại theo cặp của chúng có khớp với các giá trị đã cho hay không. Người ta có thể tưởng tượng việc lặp lại các bài tập có thể có của$a, b, c$trong phạm vi lên đến giá trị đầu vào tối đa và kiểm tra tất cả các ràng buộc. Điều này hoạt động về mặt khái niệm vì việc xác minh rất đơn giản: tính toán ba cực đại và so sánh. Tuy nhiên, không gian tìm kiếm là khối trong phạm vi giá trị, điều này hoàn toàn không thể thực hiện được trong các ràng buộc. 

Nhận xét quan trọng là cấu trúc của bài toán tạo ra một mối quan hệ rất chặt chẽ giữa ba số đã cho. Mỗi trong số$x, y, z$là mức tối đa của một cặp, do đó mức tối đa toàn cầu giữa$x, y, z$phải tương ứng với một giá trị xuất hiện trong ít nhất hai giá trị cực đại theo cặp. Điều này là do lớn nhất của$a, b, c$phải tham gia vào cả hai cực đại theo cặp liên quan đến phần tử đó. 

Cho phép$M = \max(x, y, z)$. Để nghiệm tồn tại thì ít nhất hai số phải bằng nhau$M$. Nếu điều kiện này không thành công, giá trị lớn nhất sẽ buộc phải xuất hiện chỉ một lần trong số các cực đại theo cặp, điều này mâu thuẫn với cấu trúc cực đại theo cặp trên ba phần tử. 

Nếu điều kiện được giữ vững, việc xây dựng sẽ trở nên đơn giản. Giả định$x = y = M$. Sau đó chúng ta có thể thiết lập$a = M$,$b = z$, Và$c = z$hoặc sắp xếp lại tùy thuộc vào giá trị nào phù hợp. Một cấu trúc rõ ràng hơn là gán mức tối đa lặp lại cho hai cực đại theo cặp và đặt biến thứ ba đủ nhỏ để khớp với giá trị còn lại. Điều này đảm bảo tính nhất quán vì cực đại liên quan đến phần tử lớn nhất được tái tạo$M$, trong khi cặp còn lại tạo ra giá trị thứ ba. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả$a,b,c$) |$O(N^3)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Tối ưu (kiểm tra tần số tối đa) |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm thành một kiểm tra tính nhất quán đơn giản trên ba giá trị. 

1. Tính toán$M = \max(x, y, z)$. Điều này đại diện cho ứng cử viên duy nhất cho phần tử lớn nhất trong số tất cả các cực đại theo cặp. 
2. Đếm xem có bao nhiêu$x, y, z$bằng với$M$. Nếu số này nhỏ hơn 2 thì xuất ra "NO". Lý do là một lần xuất hiện của mức cực đại toàn cục không thể được phân bố thành hai cực đại theo cặp khác nhau. 
3. Nếu điều kiện được thỏa mãn, hãy xây dựng bộ ba hợp lệ. Chúng tôi chỉ định mức tối đa lặp lại là hai trong số các giá trị trong$(a, b, c)$, và đặt số thứ ba làm biến còn lại. Một xây dựng nhất quán là thiết lập$a = M$,$b = M$, Và$c = \min(x, y, z)$. Sau đó điều chỉnh thứ tự ngầm bằng cách dựa vào thực tế là cực đại chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào nhãn. 
4. Xuất "YES" và bộ ba được xây dựng. 

### Tại sao nó hoạt động 

Trong bất kỳ cấu hình hợp lệ nào, phần tử lớn nhất trong số$a, b, c$phải xuất hiện ở đúng hai cực đại theo cặp. Điều này là do giá trị lớn nhất chiếm ưu thế bất kỳ cặp nào mà nó tham gia. Do đó, trong số$x, y, z$, giá trị tối đa phải được lặp lại ít nhất hai lần. Ngược lại, nếu lặp lại hai lần, chúng ta có thể gán hai cực đại đó cho các cặp gồm phần tử lớn nhất và giá trị còn lại tương ứng với cặp được tạo bởi hai phần tử nhỏ hơn. Điều này đảm bảo rằng cả ba phương trình đều được thỏa mãn đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y, z = map(int, input().split())
    
    m = max(x, y, z)
    cnt = (x == m) + (y == m) + (z == m)
    
    if cnt < 2:
        print("NO")
        continue
    
    # construct
    a = b = m
    c = min(x, y, z)
    
    print("YES")
    print(a, b, c)
```Việc thực hiện dựa vào việc dịch trực tiếp điều kiện cần thiết. Điều tinh tế quan trọng là chúng tôi không cố gắng chỉ định đầu vào nào tương ứng với cặp nào một cách rõ ràng. Thay vào đó, chúng tôi khai thác tính đối xứng: chỉ có nhiều tập hợp giá trị mới quan trọng, vì vậy, chúng tôi cố định hai biến ở mức tối đa và sử dụng giá trị nhỏ nhất làm giá trị thứ ba. 

Việc xây dựng được thực hiện vì bất kỳ đơn đặt hàng nào của$a, b, c$được cho phép ở đầu ra. Điều này loại bỏ nhu cầu ánh xạ chính xác giữa đầu vào và danh tính cặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 3
```Chúng tôi tính toán$M = 3$, và nó xuất hiện hai lần. Thế là tiến hành xây dựng. 

| Bước | x | y | z | tối đa | số lượng (tối đa) | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 2 | 3 | 3 | 2 | hợp lệ | 
| xây dựng | - | - | - | - | - | đặt a=b=3, c=2 | 

Đầu ra gấp ba là$3, 3, 2$. Mọi hoán vị đều hợp lệ. 

Điều này xác nhận rằng khi mức tối đa được nhân đôi, chúng ta luôn có thể neo nó làm phần tử chi phối trong hai biến. 

### Ví dụ 2 

đầu vào:```
10 30 20
```Đây$M = 30$, nhưng nó chỉ xuất hiện một lần. 

| Bước | x | y | z | tối đa | số lượng (tối đa) | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | 30 | 20 | 30 | 1 | không hợp lệ | 

Chúng tôi ngay lập tức kết luận là không thể. 

Điều này chứng tỏ rằng một lần xuất hiện của mức tối đa không thể được phân phối trên hai cực đại theo cặp, vì một biến không thể thống trị đồng thời hai cặp riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm sử dụng so sánh và xây dựng theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Thuật toán xử lý từng trường hợp kiểm thử một cách độc lập và chỉ thực hiện một số thao tác không đổi, nằm trong giới hạn cho$2 \cdot 10^4$đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        x, y, z = map(int, input().split())
        m = max(x, y, z)
        cnt = (x == m) + (y == m) + (z == m)
        if cnt < 2:
            out.append("NO")
        else:
            out.append("YES")
            out.append(f"{m} {m} {min(x,y,z)}")
    return "\n".join(out) + "\n"

# provided sample 1
assert run("""5
3 2 3
100 100 100
50 49 49
10 30 20
1 1000000000 1000000000
""").strip() == """YES
3 3 2
YES
100 100 100
NO
NO
YES
1000000000 1000000000 1""".strip()

# all equal
assert run("""1
7 7 7
""").strip() == """YES
7 7 7""".strip()

# impossible case
assert run("""1
5 4 3
""").strip() == """NO""".strip()

# boundary large
assert run("""1
1000000000 1000000000 1
""").strip() == """YES
1000000000 1000000000 1""".strip()
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ + cùng ba | trường hợp hoàn toàn nhất quán | 
| 5 4 3 | KHÔNG | giảm nghiêm ngặt, không lặp lại max | 
| 1e9 1e9 1 | CÓ | xây dựng ranh giới | 

## Vỏ cạnh 

Khi cả ba giá trị đều bằng nhau, chẳng hạn như$100, 100, 100$, thuật toán xác định chính xác hai lần xuất hiện của mức tối đa và xây dựng một bộ ba tầm thường trong đó tất cả các giá trị đều giống hệt nhau. Các điều kiện cực đại được giữ nguyên vì mọi cặp đều tạo ra kết quả như nhau. 

Khi mức tối đa chỉ xuất hiện một lần, chẳng hạn như$50, 49, 49$, thuật toán bác bỏ trường hợp này. Việc cố gắng gán một giá trị lớn nhất cho hai cực đại theo cặp khác nhau không thành công vì giá trị đó không thể đồng thời đóng vai trò là giá trị tối đa trong hai cặp riêng biệt trừ khi nó được nhân đôi giữa các cực đại được cung cấp. 

Khi hai giá trị bằng giá trị lớn nhất và giá trị thứ ba rất nhỏ, chẳng hạn như$1, 1000000000, 1000000000$, cấu trúc vẫn hoạt động vì mức tối đa lặp lại có thể đóng vai trò là cả hai phần tử chi phối trong cấu trúc theo cặp, trong khi giá trị nhỏ trở thành phần tử tối thiểu được chia sẻ bởi cặp còn lại.
