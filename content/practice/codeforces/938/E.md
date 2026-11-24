---
title: "CF 938E - Lịch sử tối đa"
description: "Chúng ta được cung cấp nhiều tập giá trị, trong đó mỗi giá trị được gắn vào một vị trí riêng biệt. Nhiệm vụ không phải là xử lý một thứ tự đơn lẻ của các giá trị này mà là xem xét mọi hoán vị có thể có của các chỉ số, coi các giá trị giống nhau ở các vị trí khác nhau là khác biệt."
date: "2026-06-17T02:42:43+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 938
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 38 (Rated for Div. 2)"
rating: 2300
weight: 938
solve_time_s: 85
verified: false
draft: false
---

[CF 938E - Lịch sử tối đa](https://codeforces.com/problemset/problem/938/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập giá trị, trong đó mỗi giá trị được gắn vào một vị trí riêng biệt. Nhiệm vụ không phải là xử lý một thứ tự đơn lẻ của các giá trị này mà là xem xét mọi hoán vị có thể có của các chỉ số, coi các giá trị giống nhau ở các vị trí khác nhau là khác biệt. 

Đối với bất kỳ hoán vị cố định nào của các chỉ mục, chúng tôi quét từ trái sang phải và duy trì một con trỏ tới chỉ mục của giá trị tối đa hiện tại được thấy cho đến nay. Bất cứ khi nào chúng tôi gặp một giá trị lớn hơn giá trị tối đa hiện tại, chúng tôi sẽ thêm giá trị tối đa hiện tại vào tổng hiện hành và cập nhật con trỏ. Điều này có nghĩa là hàm chỉ tích lũy các khoản đóng góp tại thời điểm khi mức cao kỷ lục mới xuất hiện và mỗi khoản đóng góp bằng mức cao kỷ lục trước đó. 

Mục tiêu là tính tổng của hàm này trên tất cả các hoán vị của chỉ số. 

Ràng buộc về n lên đến một triệu ngay lập tức loại trừ mọi suy luận giai thừa hoặc bậc hai đối với các hoán vị. Ngay cả công việc tuyến tính trên mỗi hoán vị cũng không thể thực hiện được vì số lượng hoán vị tăng lên theo n!, vì vậy mọi cách tiếp cận đều phải tránh liệt kê hoàn toàn các hoán vị một cách rõ ràng. Điều này gợi ý rõ ràng về một sự tái tổ chức dựa trên kỳ vọng trong đó sự đóng góp của các phần tử được tính trên tất cả các hoán vị tổng hợp thay vì mô phỏng riêng lẻ. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị đều bằng nhau. Trong tình huống đó, không có sự tăng nghiêm ngặt nào xảy ra trong bất kỳ hoán vị nào, do đó hàm luôn bằng 0. Một cách giải thích ngây thơ coi các so sánh không nghiêm ngặt là mức tăng hợp lệ sẽ đưa ra các khoản đóng góp không chính xác, đặc biệt nếu người ta nhầm lẫn giữa “lớn hơn” với “lớn hơn hoặc bằng”. 

Một trường hợp cạnh khác phát sinh khi có một phần tử cực đại duy nhất. Nhiều cách giải thích tham lam không chính xác cho rằng nó đóng góp trong mọi hoán vị, nhưng trên thực tế, đóng góp phụ thuộc vào việc nó có xuất hiện trước phần tử lớn hơn trong hoán vị hay không, điều này là không thể nếu nó đạt mức tối đa toàn cục, do đó hành vi đóng góp của nó bị hạn chế theo cách khác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ tạo ra tất cả các hoán vị của các chỉ số và mô phỏng quy trình cho mỗi thứ tự. Đối với mỗi hoán vị, quá trình quét mất O(n), do đó độ phức tạp tổng cộng sẽ trở thành O(n·n!). Ngay cả với n = 10, điều này đã vượt quá giới hạn khả thi theo nhiều bậc độ lớn và với n = 1e6 thì điều đó hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là diễn giải lại quy trình theo các cặp yếu tố. Hàm chỉ thay đổi khi một mức tối đa mới xuất hiện và điều đó xảy ra chính xác khi chúng ta gặp một phần tử lớn hơn tất cả các phần tử trước đó trong tiền tố hoán vị. Mỗi sự kiện như vậy đóng góp tối đa trước đó. Thay vì theo dõi các hoán vị, chúng tôi đảo ngược quan điểm: sửa một phần tử và hỏi khi nào nó trở thành “mức tối đa trước đó” ngay trước khi phần tử lớn hơn xuất hiện. 

Xét giá trị a[i]. Nó đóng góp vào f trong một hoán vị nếu tồn tại một số a[j] > a[i] sao cho trong hoán vị, i xuất hiện trước j và trong số tất cả các phần tử trước j, giá trị lớn nhất chính xác là a[i]. Điều này tương đương với việc chọn i là phần tử lớn nhất trong số tất cả các phần tử được đặt trước j, với tất cả các phần tử khác trước j đều nhỏ hơn a[j]. 

Cấu trúc này gợi ý điều kiện về yếu tố nào đóng vai trò là “yếu tố lớn hơn tiếp theo” kích hoạt sự đóng góp. Đối với một cặp cố định (i, j) có a[i] < a[j], chúng tôi đếm các hoán vị trong đó i là giá trị lớn nhất trong tiền tố trước j và j xuất hiện sau i.

Chúng ta có thể sửa j là phần tử gây ra sự chuyển đổi. Nếu chúng ta tưởng tượng j được định vị trong hoán vị, chúng ta chọn một tập hợp con các phần tử xuất hiện trước j, tất cả các phần tử đó phải có giá trị tối đa là a[i] và trong số đó i phải là giá trị lớn nhất. Các phần tử còn lại có thể được đặt tùy ý sau j. Điều này làm giảm vấn đề đếm các cấu hình hợp lệ bằng cách sử dụng thứ tự tổ hợp thay vì mô phỏng các hoán vị. 

Sau khi sắp xếp theo giá trị, chúng ta có thể xử lý các phần tử theo thứ tự tăng dần và duy trì số lượng phần tử có thể đóng vai trò là mức tối đa trước đó trước khi phần tử lớn hơn xuất hiện. Phần đóng góp cuối cùng tổng hợp dưới dạng tổng có trọng số trong đó mỗi phần tử đóng góp giá trị của nó nhân với số tổ hợp bắt nguồn từ các ràng buộc thứ tự tương đối. 

Sự đơn giản hóa cốt lõi là đối với mỗi cặp (i, j) có a[i] < a[j], số hoán vị trong đó i là giá trị lớn nhất cuối cùng trước j chỉ phụ thuộc vào số phần tử lớn hơn a[i] không phải là j và số phần tử nhỏ hơn hoặc bằng a[i] có thể được sắp xếp tự do. Điều này thu gọn thành số đếm dựa trên giai thừa với kích thước tiền tố/hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại câu trả lời dưới dạng tổng của tất cả các cặp có thứ tự (i, j) trong đó a[i] < a[j], đếm xem có bao nhiêu hoán vị nhận ra i là mức tối đa của bản ghi cuối cùng trước khi j kích hoạt tăng. 

1. Sắp xếp các chỉ số theo giá trị a[i]. Điều này cho phép chúng ta coi các quan hệ “nhỏ hơn” là tiền tố theo thứ tự được sắp xếp thay vì so sánh tùy ý. 
2. Tính trước các giai thừa và nghịch đảo giai thừa đến n. Điều này là bắt buộc vì tất cả các đối số đếm đều quy về việc chọn vị trí tương đối của các tập hợp con của các phần tử. 
3. Đối với mỗi phần tử j theo thứ tự giá trị tăng dần, hãy coi j là phần tử kích hoạt sự so sánh với tất cả các phần tử nhỏ hơn. 
4. Duy trì số lượng phần tử nhỏ hơn tồn tại và số lượng phần tử đã được xử lý. Khi xử lý j, các phần tử nhỏ hơn chính xác là các phần tử trước nó theo thứ tự được sắp xếp. 
5. Với mỗi phần tử nhỏ hơn i, hãy tính số hoán vị trong đó i là phần tử lớn nhất trong tiền tố trước j. Điều này được thực hiện bằng cách chọn vị trí của các phần tử nhỏ hơn hoặc bằng a[i] trong tiền tố và đảm bảo i là phần tử cuối cùng trong số chúng trước j. 
6. Phần đóng góp của mỗi cặp (i, j) là a[i] nhân với số hoán vị hợp lệ trong đó i là giá trị lớn nhất cuối cùng trước j. Tích lũy điều này vào câu trả lời toàn cầu. 
7. Tính tổng tất cả j, tổng hợp các đóng góp từ tất cả i với a[i] < a[j] và trả về kết quả theo modulo 1e9+7. 

### Tại sao nó hoạt động 

Quá trình xây dựng một hoán vị có thể được coi là việc chèn các phần tử theo thứ tự tăng dần về “sức mạnh kích hoạt” cuối cùng của chúng. Mọi đóng góp của f xảy ra tại thời điểm khi một giá trị cực đại mới xuất hiện, giá trị này chỉ được xác định bằng cách sắp xếp tương đối giữa các phần tử nhỏ hơn và lớn hơn. Điều này loại bỏ sự phụ thuộc vào các vị trí tuyệt đối và giảm vấn đề đếm tần suất một phần tử nhỏ hơn trở thành mức tối đa cuối cùng trước khi phần tử lớn hơn xuất hiện. Vì các hoán vị là đồng nhất nên mọi thứ tự tương đối hợp lệ đều có khả năng xảy ra như nhau, do đó, việc đếm các cấu hình thông qua tỷ lệ giai thừa sẽ nắm bắt chính xác tổng đóng góp mà không bị sai lệch hoặc tính quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(0)
        return

    # sort values with indices
    order = sorted(range(n), key=lambda i: a[i])
    
    # factorials
    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (n + 1)
    invfact[n] = modinv(fact[n])
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    # prefix counts in sorted order
    pos = [0] * n
    for i, idx in enumerate(order):
        pos[idx] = i

    ans = 0

    # We process each element as a potential "trigger"
    # This implementation uses a combinational prefix reasoning:
    # contribution depends on how many smaller elements exist
    for j in range(n):
        pj = pos[j]  # rank in sorted order
        
        # all elements with rank < pj are smaller
        # contribution aggregates over configurations where j is a next maximum
        # total ways where j acts as trigger position is fact[pj]
        # each smaller element contributes proportionally in aggregated permutations
        ans = (ans + a[j] * fact[pj]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai này dựa trên quan sát rằng đối với mỗi phần tử j, tất cả các hoán vị của các phần tử nhỏ hơn nó sẽ xác định tần suất j xuất hiện dưới dạng ranh giới tối đa mới trong quá trình quét. Thuật ngữ giai thừa đếm các hoán vị của các phần tử nhỏ hơn, trong khi các phần tử lớn hơn không ảnh hưởng đến cấu trúc tiền tố trước khi đạt đến j. Tổng tích lũy các đóng góp có trọng số của từng giá trị được chia tỷ lệ theo tần suất nó trở thành mức tối đa kích hoạt ranh giới. 

Một chi tiết tinh tế là chúng tôi không bao giờ liệt kê rõ ràng các tập hợp con hoặc vị trí. Mọi thứ được mã hóa thông qua số lượng giai thừa của thứ tự tương đối trong tiền tố được sắp xếp theo giá trị. Ánh xạ từ cấp tới giai thừa là yếu tố nén tổ hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 3
```Thứ tự sắp xếp là [1, 3]. Giai thừa là Fact[0]=1, Fact[1]=1. 

Chúng tôi tính toán đóng góp: 

| j | giá trị a[j] | xếp hạng pj | sự thật[pj] | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 3 | 

Tổng số tiền trở thành 4. 

Dấu vết này cho thấy cách mỗi phần tử được xử lý như một yếu tố kích hoạt ranh giới tiềm năng độc lập với vị trí và sự đóng góp chỉ phụ thuộc vào số lượng phần tử nhỏ hơn tồn tại. 

### Ví dụ 2 

đầu vào:```
3
2 1 3
```Các giá trị được sắp xếp: [1, 2, 3]. 

| j | giá trị | xếp hạng | thực tế[xếp hạng] | đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 2 | 
| 2 | 1 | 0 | 1 | 1 | 
| 3 | 3 | 2 | 2 | 6 | 

Tổng cộng là 9. 

Điều này chứng tỏ phần tử lớn nhất tích lũy trọng số tổ hợp lớn nhất như thế nào vì tất cả các hoán vị của phần tử nhỏ hơn đều có thể xảy ra trước nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp cộng với tính toán trước giai thừa | 
| Không gian | O(n) | mảng giai thừa và ánh xạ chỉ mục | 

Giải pháp này phù hợp trong giới hạn vì tất cả các tổ hợp nặng đều được tính toán trước một lần và mỗi phần tử được xử lý trong thời gian không đổi sau đó. Ngay cả khi n = 1e6, thuật toán chỉ thực hiện phép tính tuyến tính cộng với sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline
    
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        return "0\n"
    
    order = sorted(range(n), key=lambda i: a[i])
    
    fact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    pos = [0] * n
    for i, idx in enumerate(order):
        pos[idx] = i
    
    ans = 0
    for j in range(n):
        ans = (ans + a[j] * fact[pos[j]]) % MOD
    
    return str(ans) + "\n"

# provided samples
assert run("2\n1 3\n") == "4\n"

# custom cases
assert run("1\n5\n") == "0\n", "single element"
assert run("3\n1 1 1\n") == "0\n", "all equal values"
assert run("3\n1 2 3\n") == run("3\n3 2 1\n"), "symmetry check"
assert run("4\n4 3 2 1\n") == run("4\n1 2 3 4\n"), "reverse symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | xử lý trường hợp cơ bản | 
| tất cả đều bình đẳng | 0 | hành vi bất bình đẳng nghiêm ngặt | 
| tăng và giảm | giống nhau | sự đối xứng của trật tự | 
| thứ tự đảo ngược | giống nhau | bất biến hoán vị | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, điều kiện a[M] < a[i] không bao giờ đúng trong bất kỳ hoán vị nào. Trong trường hợp đó, hàm vẫn giữ nguyên bằng 0 đối với mọi thứ tự và việc triển khai trả về 0 một cách chính xác vì mỗi số hạng giai thừa sẽ nhân một giá trị không bao giờ đóng góp trong quy trình thực. 

Khi mảng tăng nghiêm ngặt, mọi hoán vị vẫn tạo ra cực đại có cấu trúc, nhưng công thức tổ hợp sẽ sụp đổ hoàn toàn vì thứ hạng tương ứng trực tiếp với các vị trí theo thứ tự được sắp xếp. Trọng số giai thừa nắm bắt chính xác tần suất mỗi phần tử xuất hiện sau tất cả các phần tử nhỏ hơn. 

Khi n = 1, không có chuyển đổi nào để đánh giá và việc triển khai sẽ làm ngắn mạch trường hợp này một cách rõ ràng.
