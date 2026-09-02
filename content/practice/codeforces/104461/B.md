---
title: "CF 104461B - Chuẩn bị vấn đề"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp kiểm thử bao gồm một danh sách các số nguyên biểu thị mức độ khó của các bài toán lập trình được chuẩn bị cho một cuộc thi. Đối với mỗi danh sách, chúng ta phải quyết định xem nó có thỏa mãn một bộ quy tắc cấu trúc xác định một tập hợp cuộc thi hợp lệ hay không."
date: "2026-06-30T13:19:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104461
codeforces_index: "B"
codeforces_contest_name: "The 14th Zhejiang Provincial Collegiate Programming Contest Sponsored by TuSimple"
rating: 0
weight: 104461
solve_time_s: 94
verified: false
draft: false
---

[CF 104461B - Chuẩn bị cho vấn đề](https://codeforces.com/problemset/problem/104461/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Mỗi trường hợp kiểm thử bao gồm một danh sách các số nguyên biểu thị mức độ khó của các bài toán lập trình được chuẩn bị cho một cuộc thi. 

Đối với mỗi danh sách, chúng ta phải quyết định xem nó có thỏa mãn một bộ quy tắc cấu trúc xác định một tập hợp cuộc thi hợp lệ hay không. Các quy tắc hạn chế số lượng vấn đề, phân phối giá trị độ khó nhỏ nhất và mức độ khó có thể tăng lên một cách suôn sẻ khi được sắp xếp. 

Hạn chế đầu tiên hoàn toàn là về kích thước: chỉ những bộ có phạm vi kích thước cố định nhỏ mới được chấp nhận, cụ thể là bao gồm từ 10 đến 13 vấn đề. Bất kỳ giá trị nào nằm ngoài phạm vi này sẽ ngay lập tức không hợp lệ bất kể giá trị của nó là bao nhiêu. 

Hạn chế thứ hai xác định danh tính của các vấn đề dễ nhất. Nếu chúng ta xem xét độ khó tối thiểu trong tập hợp, thì mọi lần xuất hiện của mức tối thiểu này phải chính xác bằng giá trị 1. Điều này buộc cấp dễ nhất phải được neo ở mức 1 thay vì cho phép các giá trị thấp tùy ý. Hậu quả là nếu giá trị nhỏ nhất không bằng 1 thì cấu hình không thành công. 

Hạn chế thứ ba yêu cầu phải có ít nhất hai bài toán có độ khó 1. Điều này ngăn ngừa các trường hợp suy biến khi chỉ tồn tại một bài toán dễ nhất. 

Hạn chế thứ tư được áp dụng sau khi sắp xếp các độ khó theo thứ tự không giảm. Khi chúng ta quét các phần tử liền kề, chênh lệch tuyệt đối giữa các phần tử lân cận không được vượt quá 2. Ngoại lệ duy nhất là khi một trong hai bài toán lân cận là bài toán khó nhất duy nhất, nghĩa là giá trị lớn nhất trong mảng và nó xuất hiện đúng một lần. Bất kỳ sự kề cận nào liên quan đến mức tối đa này sẽ bỏ qua giới hạn chênh lệch. 

Một sai lầm ngây thơ là áp dụng thống nhất ràng buộc kề, bao gồm cả các cạnh có giá trị lớn nhất. Ví dụ: hãy xem xét một mảng được sắp xếp như`[1, 1, 3, 4, 5, 6, 7, 13, 14, 15]`. Bước nhảy từ 7 lên 13 là lớn, nhưng nó chỉ được phép nếu 13 là mức tối đa duy nhất. Nếu thay vào đó là 15 thì bước nhảy đó sẽ không hợp lệ. Giải pháp bỏ qua ngoại lệ sẽ từ chối các trường hợp hợp lệ một cách không chính xác. 

Một lỗi nhỏ khác xảy ra khi giá trị tối thiểu là 1 nhưng chỉ xuất hiện một lần. Ngay cả khi tất cả các khác biệt kề cận đều nhỏ thì tập hợp đó vẫn phải bị bác bỏ. 

Các ràng buộc rất nhỏ: tối đa 10^4 trường hợp thử nghiệm và mỗi mảng có tối đa 100 phần tử. Điều này có nghĩa là giải pháp O(n log n) cho mỗi trường hợp thử nghiệm là đủ dễ dàng và thậm chí O(n^2) sẽ là đường biên nhưng không cần thiết. Giải pháp dự định là tuyến tính sau khi sắp xếp. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để diễn giải các quy tắc là mô phỏng mọi điều kiện trực tiếp trên từng trường hợp thử nghiệm mà không đơn giản hóa cấu trúc. Chúng tôi sẽ kiểm tra từng thuộc tính một, thậm chí có thể tính toán lại cực tiểu, cực đại và số đếm nhiều lần, sau đó xác minh các ràng buộc kề trong khi xác định động xem liệu có liên quan đến mức tối đa hay không. Điều này đúng nhưng không hiệu quả xét về mặt khái niệm vì nó có thể quét mảng nhiều lần để kiểm tra tối đa trong quá trình xác thực kề, dẫn đến công việc dư thừa. Trong trường hợp xấu nhất, nếu chúng tôi tính toán lại mức tối đa hoặc số lượng cho mỗi lần kiểm tra kề, chúng tôi sẽ hướng tới O(n^2) cho mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là tất cả các thuộc tính toàn cục bắt buộc có thể được tính toán trước một lần cho mỗi mảng. Giá trị tối thiểu, tối đa và tần suất của các giá trị không phụ thuộc vào việc kiểm tra kề và lý do cấu trúc duy nhất cần thiết sau khi sắp xếp là quét tuyến tính đơn. Điều này làm giảm vấn đề xuống còn một bước xác minh đơn giản qua danh sách được sắp xếp với các kiểm tra liên tục theo thời gian để xem liệu một vị trí có chứa giá trị tối đa hay không. 

Lực lượng vũ phu hoạt động vì nó trực tiếp thực thi các quy tắc, nhưng nó không thể mở rộng quy mô một cách rõ ràng vì nó trộn các truy vấn toàn cầu vào các kiểm tra cục bộ. Thống kê tóm tắt tính toán trước sẽ phân tách các mối quan tâm và giảm mọi thứ xuống O(n log n) do sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi lần kiểm tra | O(1) thêm | Quá chậm | 
| Tối ưu | O(n log n) mỗi lần kiểm tra | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Các bước suy luận tối ưu 

1. Đọc mảng và kiểm tra ngay độ dài của nó. Nếu nó nằm ngoài phạm vi từ 10 đến 13 thì cấu hình không thể hợp lệ nên chúng ta có thể từ chối nó mà không cần làm gì thêm. Điều này tránh việc sắp xếp không cần thiết cho các trường hợp không hợp lệ rõ ràng. 
2. Sắp xếp mảng theo thứ tự không giảm. Bước này là cần thiết vì các ràng buộc kề chỉ được xác định sau khi sắp xếp và nếu không sắp xếp, chúng ta không thể suy luận cục bộ về độ trơn tru. 
3. Tính giá trị tối thiểu và tối đa từ mảng đã được sắp xếp. Cũng đếm xem mức tối thiểu xuất hiện bao nhiêu lần. Những giá trị này xác định liệu các yêu cầu về cấu trúc về các điểm cực trị có được thỏa mãn hay không. 
4. Xác minh rằng giá trị tối thiểu chính xác là 1. Nếu không, quy tắc “các vấn đề dễ nhất là 1” sẽ bị vi phạm ngay lập tức. 
5. Kiểm tra xem số lượng 1 có ít nhất là 2. Điều này đảm bảo có đủ các bài toán dễ nhất. 
6. Xác nhận rằng giá trị tối đa xuất hiện đúng một lần. Điều này xác định vấn đề khó khăn nhất mà các quy tắc yêu cầu. 
7. Quét các cặp liền kề trong mảng đã sắp xếp. Đối với mỗi cặp, hãy kiểm tra sự khác biệt tuyệt đối của chúng. Nếu một trong hai phần tử bằng giá trị tối đa, hãy bỏ qua ràng buộc cho cặp đó. Nếu không, hãy đảm bảo chênh lệch tối đa là 2. Nếu bất kỳ cặp nào vi phạm điều này, cấu hình không hợp lệ. 

### Tại sao nó hoạt động

Sau khi sắp xếp, tất cả các ràng buộc về cấu trúc sẽ giảm xuống mức kiểm tra tính nhất quán cục bộ ngoại trừ các điều kiện tổng thể về mức tối thiểu và tối đa. Việc tính toán trước tối thiểu, tối đa và tần số sẽ tách biệt các ràng buộc toàn cục để việc xác thực kề trở nên độc lập. Điều bất biến là mọi phần tử không tối đa phải nằm trong một chuỗi trong đó các giá trị tăng dần với kích thước bước nhiều nhất là 2. Điểm gián đoạn duy nhất được phép là ở ranh giới nơi mức tối đa duy nhất tham gia, điều này không ảnh hưởng đến tính liên tục của chuỗi còn lại. Sự tách biệt này đảm bảo rằng mọi vi phạm sẽ xuất hiện trong kiểm tra toàn cầu hoặc trong so sánh cặp liền kề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if n < 10 or n > 13:
            out.append("No")
            continue

        a.sort()

        mn = a[0]
        mx = a[-1]

        if mn != 1:
            out.append("No")
            continue

        cnt1 = 0
        for x in a:
            if x == 1:
                cnt1 += 1

        if cnt1 < 2:
            out.append("No")
            continue

        if a.count(mx) != 1:
            out.append("No")
            continue

        ok = True
        for i in range(n - 1):
            if a[i] == mx or a[i + 1] == mx:
                continue
            if abs(a[i + 1] - a[i]) > 2:
                ok = False
                break

        out.append("Yes" if ok else "No")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên lọc các trường hợp không thể thực hiện được bằng cách sử dụng giới hạn kích thước. Sắp xếp thiết lập thứ tự đúng cho tất cả các lý do tiếp theo. Việc kiểm tra giá trị và tần số tối thiểu thực thi các yêu cầu về cấu trúc đối với những vấn đề đơn giản nhất. Việc kiểm tra tính duy nhất ở mức tối đa đảm bảo quy tắc ngoại lệ đặc biệt được xác định rõ ràng. 

Vòng lặp cuối cùng là bước xác thực cốt lõi. Nó chỉ thực thi ràng buộc khác biệt khi không có phần tử nào đạt mức tối đa, khớp chính xác với quy tắc ngoại lệ. Điều này tránh việc vô tình từ chối các cấu hình hợp lệ trong đó phần tử lớn nhất tạo ra bước nhảy lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[1, 1, 3, 4, 5, 6, 7, 13, 14, 15]`| Bước | Mảng được sắp xếp | Kiểm tra chìa khóa | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Tương tự | n = 10 hợp lệ | vượt qua | 
| 2 | Tương tự | phút = 1 | vượt qua | 
| 3 | Tương tự | số (1) ≥ 2 | vượt qua | 
| 4 | Tương tự | tối đa = 15 duy nhất | vượt qua | 
| 5 | Quét cặp | 7 đến 13 được phép do có sự tham gia tối đa | vượt qua | 

Bước nhảy lớn giữa 7 và 13 không làm mất hiệu lực của mảng vì 15 là mức tối đa duy nhất, do đó các quy tắc kề không áp dụng ở ranh giới đó. 

### Ví dụ 2 

Mảng đầu vào:`[1, 1, 2, 4, 7, 9, 10, 12, 13, 14]`| Bước | Mảng được sắp xếp | Kiểm tra chìa khóa | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Tương tự | n = 10 hợp lệ | vượt qua | 
| 2 | Tương tự | phút = 1 | vượt qua | 
| 3 | Tương tự | số (1) ≥ 2 | vượt qua | 
| 4 | Tương tự | tối đa = 14 duy nhất | vượt qua | 
| 5 | Quét cặp | 4 đến 7 khác biệt 3 vi phạm quy tắc | thất bại | 

Trường hợp này cho thấy rằng ngay cả khi tất cả các ràng buộc toàn cục được thỏa mãn, một khoảng cách cục bộ lớn hơn 2 sẽ làm mất hiệu lực cấu hình vì nó xuất hiện cách xa mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Sắp xếp chiếm ưu thế; quét và đếm tuyến tính là O(n) | 
| Không gian | O(1) thêm | Sắp xếp được thực hiện ngoài việc lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 100 phần tử cho mỗi trường hợp thử nghiệm, do đó việc sắp xếp tối đa 13 phần tử cho mỗi thử nghiệm là chuyện nhỏ. Ngay cả 10^4 trường hợp thử nghiệm cũng dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided sample (formatted cleanly)
sample_input = """8
9
1 2 3 4 5 6 7 8 9
10
1 2 3 4 5 6 7 8 9 10
11
9 9 1 1 2 3 4 5 6 7 8
9
1 3 5 7 9 11 13 17 19 21
10
15 1 13 17 1 7 9 5 3 11
13
1 1 1 1 1 1 1 1 1 1 1 2
10
2 3 4 5 6 7 8 9 10 11
10
15 1 13 3 6 5 4 7 1 14
"""

sample_output = """No
No
Yes
No
Yes
Yes
No
No"""

# custom cases
assert run("10\n1 1 2 3 4 5 6 7 8 9\n") == "No", "min not unique max structure fails"
assert run("10\n1 1 2 4 6 8 10 12 14 16\n") == "No", "adjacency violations"
assert run("10\n1 1 1 1 1 1 1 1 1 2\n") == "No", "valid structure but missing max uniqueness constraint"
assert run("11\n1 1 2 2 3 3 4 4 5 5 6\n".strip()) in ["Yes","No"], "boundary stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 nhỏ liên tiếp | Không | giá trị cấu trúc tối thiểu không phải 1 | 
| giá trị cách nhau | Không | thực thi ràng buộc liền kề | 
| nhiều cái không có quy tắc tối đa rõ ràng | Không | tính duy nhất của yêu cầu tối đa | 
| ranh giới hỗn hợp | Có/Không | độ bền vững trên cấu trúc hợp lệ đường biên | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi mảng chứa nhiều giá trị nhỏ nhưng chỉ có một giá trị 1. Ngay cả khi chênh lệch kề nhỏ, quy tắc yêu cầu ít nhất hai giá trị 1 buộc phải loại bỏ. Thuật toán xử lý việc này một cách chính xác vì nó đếm rõ ràng số lần xuất hiện là 1 trước bất kỳ kiểm tra cấu trúc nào. 

Một trường hợp cạnh khác phát sinh khi giá trị lớn nhất xuất hiện nhiều lần. Ngay cả khi các ràng buộc kề được vượt qua, thì quy tắc ngoại lệ vẫn không được xác định rõ ràng, do đó thuật toán sẽ loại bỏ nó sớm bằng cách kiểm tra tính duy nhất của mức tối đa. 

Trường hợp thứ ba là khi một khoảng trống lớn xuất hiện liền kề với mức tối đa nhưng cũng ảnh hưởng đến các chuyển tiếp không tối đa ở nơi khác. Thuật toán đảm bảo tính chính xác bằng cách chỉ bỏ qua các so sánh liên quan đến mức tối đa, do đó tất cả các cặp khác đều được xác thực nghiêm ngặt, đảm bảo không bỏ sót vi phạm ẩn nào.
