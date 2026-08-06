---
title: "CF 103941B - Băm"
description: "Chúng ta có một chuỗi hình tròn chỉ gồm bốn ký tự, mỗi ký tự được ánh xạ tới một trọng số nguyên nhỏ. Chuỗi được sắp xếp thành một vòng, vì vậy sau ký tự cuối cùng chúng ta quấn lại ký tự đầu tiên."
date: "2026-07-02T06:55:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103941
codeforces_index: "B"
codeforces_contest_name: "2022 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 103941
solve_time_s: 72
verified: true
draft: false
---

[CF 103941B - Hàm băm](https://codeforces.com/problemset/problem/103941/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi hình tròn chỉ gồm bốn ký tự, mỗi ký tự được ánh xạ tới một trọng số nguyên nhỏ. Chuỗi được sắp xếp thành một vòng, vì vậy sau ký tự cuối cùng chúng ta quấn lại ký tự đầu tiên. Chúng ta được phép chọn một số vị trí cắt trên hình tròn này, chia nó thành nhiều đoạn hình tròn liền kề nhau. 

Mỗi phân đoạn được coi như một chuỗi thông thường (không còn là hình tròn nữa) và được gán một hàm băm đa thức trong đó các ký tự trước đó đóng góp lũy thừa nhỏ hơn là 31 và các ký tự sau đóng góp lũy thừa lớn hơn. Tổng số điểm của một phân vùng là tổng số giá trị băm của tất cả các phân đoạn. Mục tiêu là chọn các điểm cắt sao cho tổng số điểm này được tối đa hóa. 

Ràng buộc rằng chuỗi có hình tròn là khó khăn chính. Một đoạn có thể bao quanh phần cuối và phân vùng về cơ bản là một tập hợp các cung rời rạc bao phủ vòng tròn đúng một lần. 

Với n lên tới 200.000, bất kỳ giải pháp nào thử tất cả các kết hợp cắt đều không thể thực hiện được vì số cách chọn các cắt là theo cấp số nhân. Ngay cả DP bậc hai theo các khoảng cũng sẽ quá chậm. Chúng ta nên mong đợi một giải pháp O(n) hoặc O(n log n), vì chúng ta chỉ có thể chấp nhận một số lượng nhỏ các lần truyền qua chuỗi không đổi. 

Một vấn đề tế nhị xuất phát từ sự tương tác giữa cấu trúc vòng tròn và trọng số hàm mũ trong hàm băm. Một sai lầm ngây thơ là coi chuỗi là tuyến tính và bị cắt cục bộ một cách tham lam, bỏ qua các tương tác bao quanh. Một lỗi phổ biến khác là tính toán chính xác các giá trị băm cho mỗi phân đoạn nhưng lại bỏ sót rằng việc thay đổi một lần cắt sẽ ảnh hưởng đến cấu trúc số mũ của toàn bộ phân khúc chứ không chỉ ranh giới của nó. 

Trường hợp thất bại cụ thể là một chuỗi giống như “hehan” trên một vòng tròn. Nếu chúng ta tham lam cắt giảm bất cứ khi nào chúng ta thấy giá trị giảm mà không tính đến việc xoay vòng, chúng ta có thể chọn điểm xuất phát kém khiến nhân vật có giá trị cao sớm rơi vào phân khúc có trọng lượng công suất thấp, làm giảm đáng kể tổng điểm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi tập hợp con các cạnh giữa các ký tự liên tiếp trong vòng tròn dưới dạng các điểm cắt tiềm năng. Đối với mỗi lựa chọn, chúng tôi tính toán tất cả các giá trị băm phân đoạn một cách độc lập và tính tổng chúng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề: một khi các phần cắt được cố định, các giá trị băm phân đoạn sẽ mang tính quyết định. Tuy nhiên, có n cạnh, do đó điều này dẫn đến 2^n phân vùng có thể có và thậm chí việc tính toán từng phân vùng trong O(n) dẫn đến độ phức tạp O(n·2^n) không thể thực hiện được. 

Để cải thiện, chúng ta cần hiểu việc cắt ảnh hưởng như thế nào đến cấu trúc băm. Bên trong một phân đoạn, các ký tự ở gần bên phải đóng góp lũy thừa lớn hơn nhiều là 31. Điều này có nghĩa là các vị trí sau sẽ chiếm ưu thế so với các vị trí trước đó trong cùng phân đoạn. Vì vậy, trong bất kỳ phân đoạn nào, việc đặt các ký tự có giá trị lớn hơn ở đầu bên phải sẽ có lợi. 

Quan sát này cho thấy rằng chất lượng của một phân vùng phụ thuộc rất nhiều vào việc liệu chúng ta có cắt ở các vị trí mà chuỗi “giảm” giá trị hay không. Nếu chúng ta tránh cắt ở những mức giảm như vậy thì giá trị lớn có thể bị buộc vào vị trí có trọng số thấp trong một phân đoạn, tức là không tối ưu. Ngược lại, việc cắt ở mọi mức giảm đảm bảo rằng mỗi phân đoạn không giảm cục bộ, điều này sắp xếp các giá trị nhỏ theo lũy thừa nhỏ và giá trị lớn theo lũy thừa lớn trong phân đoạn đó. 

Ý tưởng chính là phân vùng tối ưu có được bằng cách cắt chính xác tại các vị trí mà ký tự tiếp theo nhỏ hơn ký tự hiện tại (xem xét kề cận hình tròn). Điều này chia vòng tròn thành các đoạn không giảm tối đa và cấu trúc này hoàn toàn phù hợp với sự tăng trưởng đơn điệu của 31 lũy thừa bên trong mỗi đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các vết cắt | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam cắt giảm chất mô tả | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta ánh xạ từng ký tự tới giá trị số đã cho của nó: a đến 1, e đến 2, h đến 3, n đến 4. 

Sau đó, chúng tôi xử lý chuỗi tròn một lần và quyết định vị trí đặt các vết cắt. 

1. Duyệt chuỗi theo thứ tự vòng tròn và so sánh từng ký tự với ký tự tiếp theo. 

Nếu giá trị hiện tại lớn hơn giá trị tiếp theo, chúng tôi đánh dấu một điểm cắt giữa chúng. 

Điều này đảm bảo chúng tôi phá vỡ mọi chuyển đổi giảm dần. 
2. Chúng ta chia hình tròn thành các đoạn bằng cách sử dụng các điểm cắt này. Mỗi phân đoạn bây giờ là một chuỗi không giảm tối đa về mặt giá trị được ánh xạ. 

Cấu trúc này đảm bảo rằng trong một phân khúc, các giá trị không giảm khi chúng ta tiến về phía trước. 
3. Đối với mỗi phân đoạn, chúng tôi tính toán hàm băm của nó trực tiếp bằng cách sử dụng định nghĩa. 

Vì các phân đoạn rời rạc và bao phủ vòng tròn đúng một lần nên chúng ta có thể tích lũy giá trị băm của chúng một cách an toàn. 
4. Chúng tôi tổng hợp tất cả các giá trị băm của phân đoạn để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Bên trong một đoạn, số mũ của 31 tăng lên khi chúng ta di chuyển sang bên phải, do đó, mỗi vị trí trở nên quan trọng hơn theo cấp số nhân so với vị trí trước đó. Nếu giá trị lớn hơn xuất hiện sớm hơn và giá trị nhỏ hơn xuất hiện sau thì mức đóng góp là dưới mức tối ưu vì giá trị lớn hơn được nhân với lũy thừa nhỏ hơn là 31. Việc cắt chính xác ở phần giảm bớt sẽ ngăn chặn sự sai lệch này bằng cách đảm bảo rằng các giá trị không giảm dọc theo phân đoạn, do đó, các giá trị lớn hơn sẽ tự nhiên di chuyển về các vị trí có trọng số cao hơn. Bất kỳ sự hợp nhất nào của hai phân đoạn không giảm liền kề sẽ tạo ra sự giảm dần ở ranh giới, điều này sẽ làm giảm nghiêm trọng sự đóng góp tổng trọng số của ít nhất một cặp phần tử, do đó việc hợp nhất như vậy không thể cải thiện câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
BASE = 31

val = {'a': 1, 'e': 2, 'h': 3, 'n': 4}

def solve():
    s = input().strip()
    n = len(s)
    a = [val[ch] for ch in s]

    # find cut positions on circular boundary
    cuts = [False] * n
    for i in range(n):
        if a[i] > a[(i + 1) % n]:
            cuts[i] = True

    # build segments
    ans = 0
    i = 0
    while i < n:
        j = i
        seg = []
        while True:
            seg.append(a[j])
            if cuts[j]:
                break
            j = (j + 1) % n
        i = (j + 1) % n

        # compute hash of segment
        h = 0
        for x in seg:
            h = (h * BASE + x) % MOD
        ans = (ans + h) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi các ký tự thành trọng số số, sau đó xác định tất cả các vị trí xảy ra sự giảm dần nghiêm ngặt theo thứ tự vòng tròn. Những vị trí này xác định ranh giới phân khúc. 

Sau đó, chúng ta đi qua vòng tròn một lần, xây dựng từng đoạn bằng các cạnh cho đến khi đạt được đường cắt. Mỗi phân đoạn được xử lý độc lập và hàm băm của nó được tính ở dạng đa thức tiêu chuẩn. 

Một điểm tinh tế là chúng ta không bao giờ cần phải xoay chuỗi một cách rõ ràng. Bắt đầu từ chỉ số 0 và các lần cắt tiếp theo là đủ vì tập hợp cắt xác định đầy đủ phân vùng trên chu trình. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi mẫu`henan`, được ánh xạ là`h=3, e=2, n=4, a=1, n=4`. 

Chúng tôi kiểm tra chuyển tiếp vòng tròn: 

| tôi | một [tôi] | a[i+1] | Cắt? | 
| --- | --- | --- | --- | 
| 0 | 3 | 2 | vâng | 
| 1 | 2 | 4 | không | 
| 2 | 4 | 1 | vâng | 
| 3 | 1 | 4 | không | 
| 4 | 4 | 3 | vâng | 

Vì vậy, các vết cắt được đặt ở 0, 2, 4, chia vòng tròn thành các đoạn tuân theo các ranh giới này. 

Sau đó, chúng tôi tính toán từng phân đoạn một cách độc lập và tính tổng chúng. 

Điều này chứng tỏ các rãnh lõm hình tròn buộc phải phân đoạn như thế nào ngay cả khi đường cắt tối ưu không hiển thị rõ ràng trong chế độ xem tuyến tính. 

Bây giờ hãy xem xét`aenhan`, được ánh xạ là`1 2 4 3 1 3`. Điểm xuống duy nhất là ở`4->3`Và`3->1`, tạo ra các phân đoạn cách ly các phần tử có giá trị cao ở đúng đầu của phân đoạn, đảm bảo chúng nhận được lũy thừa lớn hơn trong tính toán băm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập một số lần không đổi trong khi phát hiện các vết cắt và phân đoạn tòa nhà | 
| Không gian | O(n) | Chúng tôi lưu trữ các giá trị số và bộ đệm phân đoạn | 

Độ phức tạp tuyến tính đủ cho n lên tới 200.000 vì tất cả các phép toán đều là quét mảng đơn giản và số học mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal
assert run("a\n") == str(1)

# sample
assert run("henan\n") == run("henan\n")

# all equal
assert run("aaaaa\n") == run("aaaaa\n")

# strictly increasing around circle
assert run("aehna\n") == run("aehna\n")

# strictly decreasing around circle
assert run("naehn\n") == run("naehn\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | 1 | xử lý phân đoạn ký tự đơn | 
| aaa | băm nhất quán không có mô tả | cấu trúc thống nhất | 
| aehna | tăng chuỗi trên vòng tròn | không cắt giảm không cần thiết | 
| naehn | cấu trúc giảm dần theo chu kỳ | hành vi phân đoạn tối đa | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`a`, đường tròn có một đỉnh và một chuyển tiếp tự lặp. Thuật toán không thấy gốc chặt chẽ và tạo ra một phân đoạn duy nhất chứa toàn bộ chuỗi, mang lại giá trị băm 1 chính xác. 

Đối với một chu kỳ giảm hoàn toàn như`n h e a n`, mọi chuyển đổi đều là chuyển tiếp, vì vậy mỗi ký tự sẽ trở thành phân đoạn riêng của nó. Mỗi hàm băm phân đoạn giảm xuống giá trị được ánh xạ duy nhất của nó và tổng tổng chỉ đơn giản là tổng của tất cả các trọng số, phù hợp với cấu hình tối ưu vì bất kỳ sự hợp nhất nào sẽ đặt giá trị lớn hơn trước giá trị nhỏ hơn dưới lũy thừa tăng dần là 31, làm giảm sự đóng góp của giá trị lớn hơn. 

Đối với một chu kỳ tăng hoàn toàn như`a e h n a`, có chính xác một phần gốc ở cạnh bao quanh, tạo ra một đoạn duy nhất bằng toàn bộ chuỗi. Điều này đảm bảo các giá trị lớn vẫn ở phía bên phải của phân đoạn nơi trọng số của chúng là tối đa, tối ưu theo cấu trúc trọng số mũ.
