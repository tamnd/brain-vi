---
title: "CF 1726A - Mainak và mảng"
description: "Chúng ta được cho một mảng các số nguyên dương. Chính xác một lần, chúng ta được phép chọn một đoạn liền kề và xoay nó theo chu kỳ với số lượng bất kỳ, điều đó có nghĩa là chúng ta có thể chọn bất kỳ phần tử nào bên trong đoạn đó và di chuyển nó đến một trong hai đầu của đoạn đó, trong khi vẫn giữ nguyên…"
date: "2026-06-15T01:49:53+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1726
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022"
rating: 900
weight: 1726
solve_time_s: 272
verified: true
draft: false
---

[CF 1726A - Mainak và Mảng](https://codeforces.com/problemset/problem/1726/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 4 phút 32 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chính xác một lần, chúng ta được phép chọn một phân đoạn liền kề và xoay nó theo chu kỳ với số lượng bất kỳ, điều đó có nghĩa là chúng ta có thể chọn bất kỳ phần tử nào bên trong phân đoạn đó và di chuyển nó đến một trong hai đầu của phân đoạn đó, trong khi vẫn giữ nguyên thứ tự tương đối của các phần tử còn lại. 

Sau khi thực hiện thao tác đơn lẻ này, chúng ta chỉ quan tâm đến sự khác biệt giữa phần tử cuối cùng và phần tử đầu tiên của toàn bộ mảng. Nhiệm vụ là chọn phân đoạn và xoay vòng sao cho tối đa hóa sự khác biệt cuối cùng này. 

Quan sát quan trọng là thao tác không bao giờ thay đổi giá trị mà chỉ sắp xếp lại chúng. Vì vậy, chúng tôi đang cố gắng thao tác phần tử gốc nào kết thúc ở vị trí 1 và phần tử nào kết thúc ở vị trí n. 

Các ràng buộc là nhỏ một cách rất được kiểm soát. Mỗi mảng có tổng cộng tối đa 2000 phần tử trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận mỗi thử nghiệm nặng bậc ba hoặc thậm chí bậc hai với hằng số lớn, nhưng vẫn cho phép giải pháp O(n^2) hoặc O(n^2 log n) một cách thoải mái. 

Một sai lầm ngây thơ là nghĩ rằng chỉ có điểm cuối mới quan trọng hoặc chỉ xem xét việc xoay toàn bộ mảng. Một sai lầm phổ biến khác là cho rằng câu trả lời tốt nhất chỉ đơn giản là max(a) trừ min(a), bỏ qua rằng chúng ta không thể tùy ý đặt cả hai cực trị ở cả hai đầu trừ khi cấu trúc xoay cho phép điều đó. 

Một trường hợp lỗi cụ thể là mảng`[2, 1, 5]`. Câu trả lời đúng nhất là 4, đạt được bằng cách xoay`[2,1,5]`ĐẾN`[5,2,1]`. Một ý tưởng ngây thơ như “đặt tối đa ở cuối và tối thiểu ở đầu một cách độc lập” không thành công vì những vị trí đó không thể kiểm soát được một cách độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi phân khúc có thể`[l, r]`và mọi vòng quay có thể. Đối với mỗi cấu hình, chúng tôi tính toán lại phần tử đầu tiên và phần tử cuối cùng, đồng thời theo dõi sự khác biệt tốt nhất. Vì có các phân đoạn O(n^2) và tối đa O(n) vòng quay trên mỗi phân đoạn và mỗi đánh giá là O(n), nên giá trị này nhanh chóng trở nên quá lớn, khoảng O(n^4) cho mỗi thử nghiệm trong mô phỏng trực tiếp. 

Sự đơn giản hóa xuất phát từ việc nhận ra rằng chúng ta không quan tâm đến sự sắp xếp bên trong của phân khúc đã chọn, chỉ phần tử nào từ phân khúc đó trở thành ranh giới đầu tiên hoặc cuối cùng của nó sau khi xoay. Xoay vòng theo chu kỳ cho phép chúng ta di chuyển bất kỳ phần tử nào trong phân đoạn đến một trong hai đầu của phân đoạn, nghĩa là tác dụng liên quan duy nhất là chúng ta có thể chọn bất kỳ cặp phần tử nào bên trong một phân đoạn và đặt chúng liền kề với các đường viền của nó. 

Vì vậy, vấn đề giảm xuống còn việc chọn hai vị trí i và j sao cho một vị trí trở thành phần tử đầu tiên và vị trí còn lại trở thành phần tử cuối cùng sau một thao tác phân đoạn. Sau đó, chúng tôi phân tích mọi cách để làm cho i trở thành vị trí 1 hoặc n và j trở thành đầu kia bằng cách sử dụng một phép quay phân đoạn đơn. 

Sự đơn giản hóa cấu trúc quan trọng là sau khi sửa phần tử đầu tiên cuối cùng, chúng ta chỉ cần xem xét phần tử cuối cùng mà chúng ta có thể đạt được tốt đến mức nào và ngược lại. Điều này dẫn đến việc kiểm tra, đối với mọi lựa chọn ranh giới có thể, giá trị cực trị nào tồn tại trong hậu tố hoặc tiền tố còn lại có thể được dịch chuyển một cách thích hợp thông qua một phân đoạn bao phủ một điểm cuối. 

Điều này thu gọn vấn đề thành quét tuyến tính với các kiểm tra liên tục theo thời gian cho mỗi vị trí, mang lại giải pháp O(n) hoặc O(n^2) tùy thuộc vào kiểu triển khai nhưng vẫn thoải mái trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào quan sát chính rằng cách duy nhất để thay đổi phần tử đầu tiên là đưa chỉ mục 1 vào phân đoạn đã chọn và tương tự, cách duy nhất để thay đổi phần tử cuối cùng là đưa chỉ mục n vào phân đoạn. Ngược lại, các vị trí đó không thay đổi. 

Điều này chia tất cả các hoạt động hợp lệ thành ba loại có ý nghĩa: các phân đoạn chạm vào đầu bên trái, các phân đoạn chạm vào đầu bên phải hoặc toàn bộ mảng. 

1. Trước tiên, hãy cân nhắc việc không chạm vào chỉ mục 1 hoặc chỉ mục n. Trong trường hợp đó, câu trả lời chỉ đơn giản là`a[n] - a[1]`, vì các điểm cuối vẫn cố định. Đây luôn là một ứng cử viên cơ bản. 
2. Tiếp theo, hãy xem xét các phân đoạn bao gồm chỉ số 1. Bằng cách chọn một phân đoạn`[1, r]`, chúng ta có thể xoay để bất kỳ phần tử nào trong tiền tố đó trở thành phần tử đầu tiên. Điều này có nghĩa là chúng ta có thể làm cho phần tử đầu tiên bằng bất kỳ phần tử nào`a[i]`Ở đâu`i ≤ r`. Phần tử cuối cùng không thay đổi trừ khi đoạn đó cũng chạm vào n. 
3. Tương tự, xét các đoạn có chỉ số n. Bằng cách chọn`[l, n]`, chúng ta có thể xoay để bất kỳ phần tử nào trong hậu tố đó trở thành phần tử cuối cùng. Điều này cho phép phần tử cuối cùng là bất kỳ`a[i]`Ở đâu`i ≥ l`, trong khi phần tử đầu tiên không thay đổi trừ khi l = 1. 
4. Cuối cùng, hãy xem xét các phân đoạn bao gồm cả hai điểm cuối, nghĩa là`[1, n]`. Trong trường hợp này, chúng ta có thể chọn bất kỳ góc quay nào của toàn bộ mảng, vì vậy, chúng ta có thể chọn bất kỳ phần tử nào làm phần tử đầu tiên và bất kỳ phần tử nào khác làm phần tử cuối cùng, miễn là chúng có thể được đặt ở hai đầu đối diện. Sự lựa chọn tối ưu ở đây chỉ đơn giản là max(a) - min(a). 
5. Trường hợp còn thiếu duy nhất là khi chúng ta thay đổi chính xác một điểm cuối. Nếu chúng ta chỉ thay đổi phần tử đầu tiên, chúng ta sẽ cố định vị trí i làm phần tử đầu tiên mới và giữ a[n] ở vị trí cuối cùng, để chúng ta tối đa hóa`a[n] - min(a[1..n-1])`trên các công trình có thể tiếp cận hợp lệ. Đối xứng, chỉ thay đổi phần tử cuối cùng sẽ mang lại`max(a[2..n]) - a[1]`. 
6. Chúng tôi tính toán tất cả các giá trị ứng viên này và lấy giá trị tối đa. 

### Tại sao nó hoạt động 

Hoạt động về cơ bản bị hạn chế bởi ranh giới phân khúc. Phép xoay chỉ hoán vị các phần tử bên trong một khối liền kề, do đó các điểm cuối bên ngoài khối vẫn cố định. Bất kỳ chiến lược nào cố gắng tối ưu hóa độc lập cả hai đầu mà không xem xét liệu một phân đoạn có thể ảnh hưởng đến cả hai hay không sẽ vượt quá các cấu hình không thể thực hiện được. Giải pháp chính xác đến từ việc phân loại xem mỗi điểm cuối nằm trong hay ngoài phân khúc đã chọn và loại bỏ các trường hợp nhất quán đó. Mọi cấu hình cuối cùng có thể đạt được đều rơi vào chính xác một trong các mẫu bao gồm điểm cuối này, do đó, việc tận dụng tối đa chúng sẽ đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(0)
        continue

    base = a[-1] - a[0]
    best = base

    mn_all = min(a)
    mx_all = max(a)

    best = max(best, mx_all - mn_all)

    mn_prefix = min(a[:-1])
    best = max(best, a[-1] - mn_prefix)

    mx_suffix = max(a[1:])
    best = max(best, mx_suffix - a[0])

    print(best)
```Mã tính toán bốn khả năng khác biệt về mặt cấu trúc xuất phát từ việc phân đoạn được chọn có ảnh hưởng đến phần tử đầu tiên, phần tử cuối cùng, cả hai hay không. Đường cơ sở giữ cho cả hai điểm cuối không thay đổi. Phạm vi toàn cầu xử lý hoàn toàn linh hoạt khi cả hai điểm cuối có thể được thao tác trong một phân đoạn đầy đủ. Các trường hợp tiền tố và hậu tố tương ứng với việc thay đổi chính xác một điểm cuối bằng cách chọn một đoạn được neo ở cạnh đó. Mỗi phép tính là thời gian không đổi cho mỗi lần kiểm tra sau khi quét đơn giản. 

Phải cẩn thận với việc cắt lát.`a[:-1]`Và`a[1:]`là an toàn vì n ≥ 1, nhưng chúng tôi bảo vệ rõ ràng n = 1 vì nếu không những lát cắt đó sẽ trở nên trống và phá vỡ các giả định tối thiểu/tối đa. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai yếu tố đầu vào: một trường hợp bất đối xứng nhỏ và một trường hợp mà các cực trị toàn cầu chiếm ưu thế. 

### Ví dụ 1 

đầu vào:`[2, 1, 5]`| Bước | cơ sở (a[n]-a[1]) | tiền tố tối thiểu | hậu tố tối đa | tối đa toàn cầu | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | - | - | - | 3 | 
| cập nhật tiền tố | 3 | 1 | - | - | 3 | 
| cập nhật hậu tố | 3 | 1 | 5 | - | 4 | 
| cập nhật toàn cầu | 3 | 1 | 5 | 4 | 4 | 

Hoạt động hậu tố cho phép chúng ta cải thiện phần tử cuối cùng trong khi vẫn giữ cố định phần tử đầu tiên, tạo ra 5 − 1 = 4, phù hợp với phép quay tối ưu. 

### Ví dụ 2 

đầu vào:`[1, 3, 9, 11, 5, 7]`| Bước | căn cứ | tiền tố tối thiểu | hậu tố tối đa | toàn cầu | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | - | - | - | 
| tiền tố | 6 | 1 | - | - | 
| hậu tố | 6 | 1 | 7 | - | 
| toàn cầu | 6 | 1 | 7 | 10 | 

Cấu hình tốt nhất đến từ việc sử dụng toàn dải, cho 11 − 1 = 10. 

Những dấu vết này cho thấy mỗi trường hợp cấu trúc tương ứng với một cách khác nhau mà một vòng quay đơn lẻ có thể tương tác với các điểm cuối như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi trường hợp sử dụng số lần quét tuyến tính không đổi trên mảng | 
| Không gian | O(1) thêm | Chỉ có một số biến được duy trì bên cạnh việc lưu trữ đầu vào | 

Tổng n qua các bài kiểm tra tối đa là 2000, do đó, việc quét tuyến tính cho mỗi bài kiểm tra dễ dàng nằm trong giới hạn, ngay cả khi sử dụng Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            out.append("0")
            continue
        base = a[-1] - a[0]
        best = base
        best = max(best, max(a) - min(a))
        best = max(best, a[-1] - min(a[:-1]))
        best = max(best, max(a[1:]) - a[0])
        out.append(str(best))
    return "\n".join(out)

# provided samples
assert run("""5
6
1 3 9 11 5 7
1
20
3
9 99 999
4
2 1 8 1
3
2 1 5
""") == """10
0
990
7
4"""

# custom cases
assert run("""1
1
42
""") == "0"

assert run("""1
2
1 100
""") == "99"

assert run("""1
3
5 5 5
""") == "0"

assert run("""1
4
10 1 2 3
""") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 | trường hợp cạnh phần tử đơn | 
| [1.100] | 99 | hiệu ứng hoán đổi không tầm thường nhỏ nhất | 
| tất cả đều bình đẳng | 0 | không thể cải thiện được | 
| tiền tố lệch min | 9 | tính chính xác tương tác tiền tố/hậu tố | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, thuật toán trả về 0 vì mỗi phép quay khiến mảng không thay đổi và sự khác biệt giữa các điểm cuối giống hệt nhau là bằng 0. Mã này xử lý rõ ràng điều này trước khi cắt, tránh các hoạt động tối thiểu/tối đa không hợp lệ. 

Đối với một mảng tăng đơn điệu như`[1,2,3,4]`, cách tốt nhất là xoay toàn bộ mảng đến vị trí 4 ở đầu và 1 ở cuối, cho kết quả là 3. Trường hợp tối đa toàn cầu nắm bắt điều này trực tiếp thông qua`max(a) - min(a)`. 

Đối với các mảng trong đó việc cải thiện tối ưu chỉ yêu cầu thay đổi một điểm cuối, chẳng hạn như`[5,1,4,3]`, các trường hợp hậu tố và tiền tố sẽ xác định chính xác mức độ có thể cải thiện ranh giới mà không yêu cầu cả hai điểm cuối phải di chuyển đồng thời.
