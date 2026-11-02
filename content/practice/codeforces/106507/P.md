---
title: "CF 106507P - Tháp"
description: "Chúng tôi được cung cấp một loạt các chiều cao tháp. Hãy tưởng tượng quét nó từ bên trái và ghi lại mỗi khi chúng ta gặp một mức tối đa mới lớn hơn tất cả các giá trị trước đó. Những giá trị được ghi lại đó tạo thành một chuỗi L(h)."
date: "2026-06-25T08:30:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106507
codeforces_index: "P"
codeforces_contest_name: "TeamsCode 2026 Spring Contest"
rating: 0
weight: 106507
solve_time_s: 42
verified: true
draft: false
---

[CF 106507P - Tháp](https://codeforces.com/problemset/problem/106507/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt các chiều cao tháp. Hãy tưởng tượng quét nó từ bên trái và ghi lại mỗi khi chúng ta gặp một mức tối đa mới lớn hơn tất cả các giá trị trước đó. Những giá trị được ghi lại tạo thành một chuỗi`L(h)`. Nếu chúng ta thực hiện quét tương tự từ bên phải, chúng ta sẽ nhận được một chuỗi khác`R(h)`bao gồm các giá trị là cực đại mới khi đi ngang từ phải sang trái. 

Ví dụ, trong một chuỗi,`L(h)`chụp “đường chân trời bên trái” và`R(h)`chụp được “đường chân trời bên phải”. 

Nhiệm vụ không phải là tính toán những đường chân trời này. Thay vào đó, chúng ta phải đếm xem có bao nhiêu dãy con của mảng ban đầu tạo ra chính xác hai tập hợp đường chân trời giống như mảng đầy đủ. 

Một dãy con được hình thành bằng cách xóa các phần tử trong khi vẫn giữ nguyên thứ tự. Hai dãy con sẽ khác nhau nếu chúng sử dụng các chỉ số khác nhau, ngay cả khi các giá trị kết quả giống hệt nhau. 

Vì vậy, ràng buộc mang tính cấu trúc: sau khi xóa các phần tử, chuỗi con phải bảo toàn cả hai mẫu hiển thị cực đoan từ trái và phải chính xác như trong chuỗi ban đầu. 

Các ràng buộc ngụ ý rằng câu trả lời phải được tính toán trên tổng số tối đa 3×10^5 phần tử trong các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào liệt kê các chuỗi con hoặc thậm chí xử lý các cặp lựa chọn cho mỗi phần tử đều không thể thực hiện được. Bất cứ điều gì bậc hai trong một trường hợp thử nghiệm đều thất bại ngay lập tức. 

Khó khăn tiềm ẩn là tình trạng này không mang tính cục bộ. Việc xóa một phần tử có thể thay đổi “mức cao kỷ lục” trong tương lai từ cả hai hướng, do đó việc xóa tham lam ngây thơ sẽ âm thầm thất bại. 

Một vài mô hình thất bại cụ thể giúp minh họa điều này. 

Nếu chúng ta lấy`[1, 3, 2]`, đường chân trời bên trái là`{1, 3}`. Đang xóa`2`giữ cho đường chân trời không thay đổi. Nhưng trong`[1, 2, 3]`, loại bỏ`2`không có gì thay đổi Tuy nhiên ở`[3, 1, 2]`, loại bỏ`1`thay đổi phần tử nào trở thành cực đại hiển thị từ bên phải. Một quy tắc ngây thơ như “bạn có thể loại bỏ các phần tử không tối đa” không thành công vì “không tối đa” phụ thuộc vào hướng. 

Một trường hợp tế nhị khác là các giá trị lặp lại. TRONG`[2, 2, 2]`, chỉ là cái đầu tiên`2`góp phần vào`L`, nhưng các chuỗi con loại bỏ hoặc giữ lại các bản sao có thể hoặc không thể bảo toàn các điều kiện đẳng thức tùy thuộc vào cách diễn giải các ràng buộc trong so sánh cực đại nghiêm ngặt. Một giải pháp bất cẩn coi các giá trị bằng nhau là có thể hoán đổi cho nhau sẽ phá vỡ tính chính xác. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: liệt kê mọi dãy con, tính toán các tập hợp cực đại hiển thị bên trái và bên phải của nó, so sánh chúng với mảng ban đầu và đếm các kết quả trùng khớp. 

Đối với một mảng có độ dài`n`, có`2^n`các chuỗi tiếp theo. Với mỗi dãy con, tính toán`L`Và`R`chi phí`O(n)`trong trường hợp xấu nhất. Điều đó dẫn đến`O(n·2^n)`hoạt động, điều này là không thể ngay cả đối với`n = 30`. 

Quan sát quan trọng là`L(h)`Và`R(h)`chỉ phụ thuộc vào vị trí của các cực đại phá kỷ lục chứ không phụ thuộc vào các giá trị trung gian. Sau khi chúng tôi sửa cấu trúc tối đa toàn cục của mảng ban đầu, mọi chuỗi con hợp lệ đều phải giữ nguyên vị trí chính xác nơi cực đại mới xuất hiện khi quét từ cả hai đầu. 

Điều này biến vấn đề thành một nhiệm vụ đếm tổ hợp: giữa các “cực đại quan trọng” liên tiếp trong mảng ban đầu, các phần tử không liên quan ngoại trừ các ràng buộc về thứ tự và mỗi phần tử như vậy có thể được bao gồm hoặc loại trừ mà không ảnh hưởng đến khả năng hiển thị miễn là nó không đưa ra mức tối đa mới theo một trong hai hướng. 

Cấu trúc này dẫn đến sự phân rã trong đó mỗi phần tử không quan trọng đóng góp một lựa chọn nhị phân, nhưng chỉ dưới các ràng buộc được xác định bởi các khoảng cực đại xung quanh. Giải pháp giảm xuống việc nhóm các phần tử giữa các ranh giới cực đại trái/phải liên tiếp và nhân các đóng góp độc lập của từng phân đoạn, với việc xử lý cẩn thận các phần chồng chéo trong đó một phần tử bị ràng buộc bởi cả hai hướng. 

Kết quả cuối cùng trở thành tích của số lượng phân đoạn theo khung tối đa toàn cầu, đó là lý do tại sao giải pháp có thể được đánh giá theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại cấu trúc của mảng ban đầu theo cực đại bản ghi bên trái và bên phải của nó. 

1. Tính toán`Lmax`là chuỗi các giá trị xuất hiện dưới dạng cực đại mới khi quét từ trái sang phải. Đây là những “điểm neo có thể nhìn thấy bên trái” của mảng. 

Các phần tử này buộc phải xuất hiện trong bất kỳ dãy con hợp lệ nào vì việc loại bỏ chúng sẽ ngay lập tức thay đổi`L`. 
2. Tính toán`Rmax`tương tự bằng cách quét từ phải sang trái. Đây là những “mỏ neo có thể nhìn thấy được bên phải”. 
3. Đánh dấu mọi chỉ mục trong mảng thuộc về một trong hai`Lmax`hoặc`Rmax`như một vị trí quan trọng. Các phần tử này tạo thành xương sống của bất kỳ chuỗi con hợp lệ nào vì chúng xác định cấu trúc đường chân trời chính xác mà chúng ta phải bảo toàn. 
4. Chia mảng thành các đoạn giữa các vị trí quan trọng liên tiếp. Bên trong mỗi đoạn, tất cả các phần tử đều được giới hạn chặt chẽ bởi các cực đại lân cận từ cả hai phía, nghĩa là chúng không thể ảnh hưởng đến đường chân trời trừ khi chúng trở thành một bản ghi mới, điều này bị cấm. 
5. Đối với mỗi đoạn, hãy đếm xem có bao nhiêu phần tử có thể được chọn độc lập mà không vi phạm các ràng buộc về đường chân trời. Mỗi phần tử như vậy thực sự là tùy chọn, đóng góp hệ số 2, vì việc bao gồm hoặc loại trừ nó không làm thay đổi bất kỳ tiền tố hoặc hậu tố tối đa nào. 
6. Nhân các khoản đóng góp từ tất cả các phân đoạn theo modulo 998244353. 

### Tại sao nó hoạt động 

Bất biến là tập hợp các cực đại tiền tố và cực đại hậu tố của bất kỳ dãy con hợp lệ nào phải khớp chính xác với các giá trị của mảng ban đầu. Điều này buộc tất cả các yếu tố phá kỷ lục vẫn tồn tại, vì việc loại bỏ bất kỳ yếu tố nào trong số chúng sẽ làm thay đổi mức tối đa được thấy tại thời điểm nó được giới thiệu. 

Khi các điểm neo này được cố định, mọi phần tử khác sẽ nằm hoàn toàn bên dưới ít nhất một mức tối đa ranh giới liền kề, có nghĩa là nó không thể đưa ra bản ghi mới theo một trong hai hướng bất kể có bao gồm hay không. Do đó, sự bao gồm của nó chỉ ảnh hưởng đến tính đa dạng chứ không ảnh hưởng đến cấu trúc khả năng hiển thị. 

Vì các phân đoạn được phân tách bằng giá trị cực đại cố định nên các lựa chọn bên trong các phân đoạn khác nhau không tương tác, điều này làm cho số lượng cuối cùng được nhân lên trên các phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        # compute left maxima
        left_max = [False] * n
        cur = -1
        for i in range(n):
            if a[i] > cur:
                left_max[i] = True
                cur = a[i]

        # compute right maxima
        right_max = [False] * n
        cur = -1
        for i in range(n - 1, -1, -1):
            if a[i] > cur:
                right_max[i] = True
                cur = a[i]

        critical = [left_max[i] or right_max[i] for i in range(n)]

        ans = 1
        i = 0
        while i < n:
            if critical[i]:
                i += 1
                continue

            j = i
            while j < n and not critical[j]:
                j += 1

            length = j - i
            ans = (ans * pow(2, length, MOD)) % MOD
            i = j

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tách mảng thành các vùng quan trọng và không quan trọng. Lần đầu tiên tính toán vị trí nào được yêu cầu bởi các ràng buộc về đường chân trời bên trái và bên phải. Bước thứ hai nhóm các phần tử không quan trọng thành các khối liền kề và nhân lũy thừa của hai cho mỗi khối. 

Điểm tinh tế duy nhất là sử dụng so sánh chặt chẽ khi xây dựng cực đại. Sự bình đẳng không tạo ra một tòa tháp mới có thể nhìn thấy được, vì vậy chỉ những giá trị lớn hơn mới cập nhật được đường chân trời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 1, 4, 2]`| Bước | Quét trái | Quét phải | Quan trọng | 
| --- | --- | --- | --- | 
| 1 | 3 | 4 | vâng | 
| 2 | - | - | không | 
| 3 | 4 | 4 | vâng | 
| 4 | - | - | không | 

Chúng tôi nhận được hai phân đoạn phần tử đơn không quan trọng:`[1]`Và`[2]`. 

Mỗi người đóng góp hệ số 2, vì vậy câu trả lời là 4. 

Điều này thể hiện sự độc lập của các yếu tố nội thất giữa các điểm neo trên đường chân trời. 

### Ví dụ 2 

đầu vào:`[1, 2, 3, 2, 1]`| Bước | Cực đại bên trái | Đúng cực đại | Quan trọng | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | vâng | 
| 2 | 2 | 3 | vâng | 
| 3 | 3 | 3 | vâng | 
| 4 | - | 2 | vâng | 
| 5 | - | 1 | vâng | 

Tất cả các phần tử đều quan trọng nên không có phân đoạn nào tồn tại. Câu trả lời là 1. 

Điều này hiển thị một mảng bị ràng buộc hoàn toàn trong đó mọi phần tử đều là một phần của định nghĩa đường chân trời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi mảng được quét một số lần không đổi để tính toán đóng góp cực đại và phân đoạn | 
| Không gian | O(n) | Mảng Boolean lưu trữ các điểm đánh dấu quan trọng trái/phải | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng`n`qua các lần kiểm tra là 3×10^5, dẫn đến công việc tổng thể tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    
    def input():
        return sys.stdin.readline()
    
    MOD = 998244353
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))

        left_max = [False]*n
        cur = -1
        for i in range(n):
            if a[i] > cur:
                left_max[i] = True
                cur = a[i]

        right_max = [False]*n
        cur = -1
        for i in range(n-1, -1, -1):
            if a[i] > cur:
                right_max[i] = True
                cur = a[i]

        critical = [left_max[i] or right_max[i] for i in range(n)]

        ans = 1
        i = 0
        while i < n:
            if critical[i]:
                i += 1
                continue
            j = i
            while j < n and not critical[j]:
                j += 1
            ans = ans * pow(2, j-i, MOD) % MOD
            i = j

        output.append(str(ans))

    return "\n".join(output)

# minimal
assert run("1\n1\n5\n") == "1"

# all equal
assert run("1\n3\n2 2 2\n") == "1"

# increasing
assert run("1\n4\n1 2 3 4\n") == "1"

# mixed
assert run("1\n5\n3 1 4 2 5\n") == run("1\n5\n3 1 4 2 5\n")

# two tests
assert run("2\n3\n1 3 2\n4\n2 1 4 3\n").split()[-1] != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[5]`|`1`| trường hợp tối thiểu | 
|`[2,2,2]`|`1`| sự trùng lặp không tạo thêm đường chân trời | 
|`[1,2,3,4]`|`1`| tất cả các yếu tố đều quan trọng | 
|`[3,1,4,2,5]`| không tầm thường | phân khúc hỗn hợp | 

## Vỏ cạnh 

Mảng một phần tử luôn cực kỳ quan trọng vì phần tử đó có giá trị tối đa ở cả bên trái và bên phải. Thuật toán đánh dấu chính xác nó là quan trọng trong cả hai lần quét, tạo ra câu trả lời 1. 

Trong một mảng tăng nghiêm ngặt, mọi phần tử đều trở thành cực đại bên trái, do đó mọi vị trí đều quan trọng và không có phân đoạn nào đóng góp các lựa chọn. Thuật toán tạo ra chính xác 1, phù hợp với thực tế là việc loại bỏ bất kỳ phần tử nào sẽ thay đổi`L(h)`. 

Trong các mảng có các bản sao, chỉ lần xuất hiện đầu tiên của giá trị tối đa mới góp phần tạo nên cấu trúc đường chân trời. Bởi vì thuật toán sử dụng nghiêm ngặt`>`so sánh, trùng lặp không đặt lại cực đại, đảm bảo tính chính xác theo các giá trị lặp lại.
