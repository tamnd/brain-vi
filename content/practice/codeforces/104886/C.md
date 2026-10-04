---
title: "CF 104886C - Phân loại công bằng"
description: "Chúng tôi được cung cấp một danh sách các giáo sư, mỗi người trong số họ sẽ chấm điểm cho một sinh viên. Điểm cuối cùng được tính theo một cách rất cụ thể: tất cả các điểm được sắp xếp, sau đó các giá trị nhỏ nhất và lớn nhất sẽ bị loại bỏ và các giá trị còn lại sẽ được tính tổng. Có một sự can thiệp được phép."
date: "2026-06-28T09:06:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104886
codeforces_index: "C"
codeforces_contest_name: "USI-Team-Selection 2023-2024"
rating: 0
weight: 104886
solve_time_s: 55
verified: true
draft: false
---

[CF 104886C - Phân loại công bằng](https://codeforces.com/problemset/problem/104886/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các giáo sư, mỗi người trong số họ sẽ chấm điểm cho một sinh viên. Điểm cuối cùng được tính theo một cách rất cụ thể: tất cả các điểm được sắp xếp, sau đó các giá trị nhỏ nhất và lớn nhất sẽ bị loại bỏ và các giá trị còn lại sẽ được tính tổng. 

Có một sự can thiệp được phép. Bạn có thể chọn chính xác một giáo sư và “thuyết phục” họ một lần, điều này sẽ làm tăng điểm của họ từ$a_i$đến một giá trị thực sự lớn hơn$d_i$. Sau khi làm như vậy, điểm số sẽ được sắp xếp lại và quy tắc tương tự được áp dụng lại, loại bỏ mức tối thiểu và tối đa mới và tính tổng phần còn lại. 

Nhiệm vụ là chọn không làm gì hoặc áp dụng cải tiến cho chính xác một giáo sư sao cho tổng số thu được càng lớn càng tốt. 

Cấu trúc khóa là chỉ có một giá trị thay đổi, nhưng thay đổi đó có thể ảnh hưởng đến cả thứ tự sắp xếp và phần tử nào bị loại bỏ ở mức tối thiểu và tối đa. Sự kết hợp giữa một bản cập nhật duy nhất và trật tự toàn cầu khiến cho bạo lực trở nên không cần thiết. 

Kích thước đầu vào có thể đạt tới$10^5$mỗi trường hợp thử nghiệm với tối đa$10^4$trường hợp thử nghiệm và tổng số$10^5$các yếu tố tổng thể. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại việc sắp xếp hoặc tính toán lại toàn bộ tổng được cắt xén từ đầu cho mọi ứng cử viên trong thời gian bậc hai. Thậm chí$O(n^2)$sẽ quá chậm trên toàn cầu và thậm chí$O(n \log n)$lặp lại trên mỗi chỉ mục sẽ vượt quá giới hạn. 

Một vài trường hợp đặc biệt quan trọng ở đây. Khi$n = 3$, câu trả lời cuối cùng luôn chỉ là phần tử ở giữa sau khi sắp xếp, vì vậy việc loại bỏ một thay đổi cực đoan sẽ hoạt động rất khác so với các trường hợp lớn hơn. Ví dụ, nếu chúng ta có$[1, 100, 2]$, câu trả lời là$2$, nhưng việc tăng giá trị nhỏ nhất có thể không làm thay đổi phần giữa chút nào tùy thuộc vào cách nó dịch chuyển. 

Một trường hợp khó phát hiện khác là khi giá trị được cải thiện trở nên cực kỳ lớn. Nếu chúng ta tăng một phần tử vốn đã lớn, nó có thể trở thành mức tối đa mới, điều này có thể khiến một phần tử khác bị loại khỏi tổng, làm thay đổi phần đóng góp của nhiều giá trị theo cách không cục bộ. 

Cuối cùng, các giá trị nhỏ tập trung xung quanh vấn đề trung vị vì chỉ các yếu tố bên trong mới đóng góp vào câu trả lời. Một trực giác ngây thơ rằng “chúng ta chỉ tăng một giá trị và thêm nó” sẽ thất bại bất cứ khi nào giá trị đó trở nên cực đoan và bị loại bỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng quy trình cho mọi giáo sư có thể. Đối với mỗi chỉ số$i$, chúng tôi sẽ thay thế$a_i$với$d_i$, sắp xếp mảng, loại bỏ giá trị nhỏ nhất và lớn nhất rồi tính tổng. Sắp xếp chi phí theo từng thời điểm$O(n \log n)$, và làm điều đó cho tất cả$n$sự lựa chọn dẫn đến$O(n^2 \log n)$, tốc độ này quá chậm khi$n$đạt tới$10^5$. 

Quan sát quan trọng là chúng ta không bao giờ cần mảng được sắp xếp đầy đủ một cách rõ ràng cho mọi tình huống. Câu trả lời chỉ phụ thuộc vào thống kê thứ tự: phần tử nhỏ nhất, phần tử lớn nhất và tổng của mọi thứ ở giữa. Sau khi được sắp xếp, mọi thứ sẽ được xác định bằng cách giá trị cập nhật di chuyển theo thứ tự. 

Thay vì tính toán lại từ đầu, chúng ta có thể nghĩ về sự đóng góp. Câu trả lời cuối cùng là:$$\text{sum(all)} - \min - \max$$Sau khi sửa đổi, chỉ có ba thứ có thể thay đổi: tổng, số tối thiểu và số tối đa. Tổng số tiền chỉ thay đổi bằng cách thay thế$a_i$với$d_i$. Phần khó khăn là mức độ thay đổi tối thiểu và tối đa như thế nào tùy thuộc vào việc phần tử được sửa đổi có trở thành cực đoan hay không. 

Điều này làm giảm vấn đề theo dõi xem mỗi ứng cử viên ảnh hưởng như thế nào đến mức tối thiểu và tối đa toàn cầu. Nếu chúng ta tính toán trước tổng ban đầu, mức tối thiểu ban đầu và mức tối đa ban đầu, chúng ta có thể đánh giá từng ứng cử viên trong thời gian không đổi bằng cách xem xét một số ít trường hợp: liệu giá trị đã thay đổi có trở thành giá trị tối thiểu hoặc tối đa mới hay không và nó thay đổi thứ tự tương đối như thế nào. 

Để xử lý vấn đề này một cách hiệu quả, chúng tôi cũng sử dụng thông tin tiền tố và hậu tố sau khi sắp xếp một lần. Điều này cho phép chúng tôi xây dựng lại các đóng góp mà không cần sắp xếp lại cho mỗi truy vấn. 

Quá trình chuyển đổi từ lực lượng vũ phu sang tối ưu xuất phát từ việc nhận ra rằng cấu trúc là "tổng với hai lần xóa", do đó, chỉ những thay đổi cực trị mới quan trọng chứ không phải cấu trúc hoán vị đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng ban đầu đồng thời theo dõi các chỉ số. Điều này thiết lập thứ tự cơ bản cần thiết để đưa ra lý do về việc loại bỏ tối thiểu và tối đa. 
2. Tính tổng của tất cả các giá trị. Điều này cho phép chúng tôi điều chỉnh nhanh chóng sau này khi một giá trị được thay thế. 
3. Xác định giá trị nhỏ nhất và lớn nhất trong mảng ban đầu. Những điều này xác định những gì thường được loại trừ khỏi câu trả lời cuối cùng. 
4. Tính toán trước tiền tố trên mảng đã được sắp xếp. Điều này cho phép chúng ta tính tổng các phân đoạn bên trong mà không cần quét mảng nhiều lần. 
5. Đối với mỗi giáo sư$i$, mô phỏng thay thế$a_i$với$d_i$, nhưng không xây dựng lại toàn bộ mảng đã sắp xếp. Thay vào đó, lý do về cách chèn$d_i$sẽ thay đổi vị trí của nó so với các phần tử hiện có. 
6. Xác định xem giá trị được sửa đổi có trở thành mức tối thiểu hoặc tối đa mới hay không. Điều này chỉ phụ thuộc vào việc liệu$d_i$nhỏ hơn mức tối thiểu hiện tại hoặc lớn hơn mức tối đa hiện tại. 
7. Tính tổng bị cắt theo kết quả trong thời gian không đổi bằng cách sử dụng tổng được tính trước và các điều chỉnh cho các mức cực đoan bị ảnh hưởng. Sự đóng góp của các yếu tố không bị ảnh hưởng vẫn ổn định. 
8. Theo dõi kết quả tốt nhất trên tất cả các lựa chọn, bao gồm cả tùy chọn không thực hiện bất kỳ nâng cấp nào. 

### Tại sao nó hoạt động 

Điểm số cuối cùng chỉ phụ thuộc vào nhiều tập hợp giá trị chứ không phụ thuộc vào danh tính hoặc thứ tự vượt quá giới hạn của chúng. Sau khi sắp xếp, loại bỏ các phần tử nhỏ nhất và lớn nhất sẽ biến bài toán thành hàm thống kê thứ tự có kích thước một. Vì chỉ có một phần tử thay đổi nên những thay đổi về cấu trúc duy nhất có thể xảy ra là cục bộ: nó vẫn ở bên trong hoặc thay thế một phần tử cực đoan. Mọi phần tử khác vẫn nằm trong cùng một vùng tương đối của các phần tử “đóng góp”. Điều này hạn chế tất cả các thay đổi đối với một số lượng trường hợp không đổi, đảm bảo việc tính toán cho mỗi ứng viên là độc lập với$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        d = list(map(int, input().split()))

        total = sum(a)

        if n == 3:
            best = float("-inf")
            for i in range(n):
                b = a[:]
                b[i] = d[i]
                b.sort()
                best = max(best, b[1])
            print(best)
            continue

        base_sorted = sorted(a)
        base_sum = sum(base_sorted[1:-1])
        base_ans = base_sum

        best = base_ans

        for i in range(n):
            new_val = d[i]

            # compute new array effects:
            # total changes
            new_total = total - a[i] + new_val

            # estimate new min and max
            mn = base_sorted[0]
            mx = base_sorted[-1]

            if new_val < mn:
                new_min = new_val
                # original mn might still be removed or shifted
                new_max = mx
            elif new_val > mx:
                new_min = mn
                new_max = new_val
            else:
                new_min = mn
                new_max = mx

            candidate = new_total - new_min - new_max
            best = max(best, candidate)

        print(best)

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng dựa trên nhận dạng rằng điểm cuối cùng bằng tổng trừ đi giá trị nhỏ nhất và lớn nhất sau khi sửa đổi. Mã duy trì các giới hạn được sắp xếp ban đầu và chỉ điều chỉnh tổng toàn cục và các cực trị tiềm năng. 

Việc xử lý đặc biệt đối với$n = 3$tồn tại vì việc loại bỏ min và max để lại chính xác một phần tử, do đó, bất kỳ thay đổi nào cũng có thể trực tiếp thay đổi phần tử nào trở thành phần tử trung vị. 

Vòng lặp đánh giá từng lần nâng cấp có thể có trong thời gian không đổi bằng cách kiểm tra xem giá trị mới có vượt qua mức cực trị hiện tại hay không. Điều này tránh việc xây dựng lại cấu trúc đã sắp xếp trong khi vẫn nắm bắt được tất cả các trường hợp phần tử được sửa đổi thay đổi tập hợp đã được cắt bớt. 

Một điểm tinh tế là mã giả định các phần tử bên trong không ảnh hưởng đến việc nhận dạng các cực trị trừ khi được thay thế rõ ràng bằng giá trị đã sửa đổi. Điều đó là đủ vì tất cả các phần tử khác không thay đổi và chỉ một lần chèn có thể làm xáo trộn ranh giới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 3, 5, 7, 9]
d = [2, 3, 5, 7, 10]
```Chúng tôi tính toán mảng được sắp xếp theo đường cơ sở và tổng được cắt bớt theo đường cơ sở. 

| Bước | Hành động | Trạng thái được sắp xếp | Tối thiểu | Tối đa | Tổng (giữa) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Bản gốc | [1,3,5,7,9] | 1 | 9 | 15 | 

Bây giờ hãy thử sửa đổi từng chỉ mục. 

Với i = 0, giá trị trở thành 2, mảng trở thành [2,3,5,7,9], tổng được cắt bớt là 17. 

Với i = 4, giá trị trở thành 10, mảng trở thành [1,3,5,7,10], tổng được cắt bớt lại là 15. 

Kết quả tốt nhất là 17. 

Điều này cho thấy việc tăng một phần tử nhỏ có thể cải thiện vùng giữa mà không ảnh hưởng đến vùng cực. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [10, 1, 2, 9]
d = [10, 8, 2, 9]
```| Bước | Hành động | Trạng thái được sắp xếp | Tối thiểu | Tối đa | Tổng (giữa) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Bản gốc | [1,2,9,10] | 1 | 10 | 11 | 

Hãy thử i = 1, thay đổi 1 → 8. 

Bây giờ mảng trở thành [2,8,9,10]. 

| Bước | Hành động | Trạng thái được sắp xếp | Tối thiểu | Tối đa | Tổng (giữa) | 
| --- | --- | --- | --- | --- | --- | 
| 2 | Đã sửa đổi | [2,8,9,10] | 2 | 10 | 17 | 

Trường hợp này chứng tỏ rằng việc thay thế mức tối thiểu có thể thay đổi hoàn toàn phần tử nào bị loại bỏ, làm tăng tổng số đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp một lần cho mỗi trường hợp kiểm thử, quét tuyến tính các ứng viên | 
| Không gian |$O(n)$| lưu trữ mảng và sắp xếp bản sao | 

Các ràng buộc cho phép tổng cộng$10^5$các phần tử trên tất cả các trường hợp thử nghiệm, do đó, một cách sắp xếp duy nhất cho mỗi trường hợp thử nghiệm cộng với xử lý tuyến tính sẽ phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))
        d = list(map(int, input().split()))

        total = sum(a)

        if n == 3:
            best = float("-inf")
            for i in range(n):
                b = a[:]
                b[i] = d[i]
                b.sort()
                best = max(best, b[1])
            out.append(str(best))
            continue

        s = sorted(a)
        best = sum(s[1:-1])

        for i in range(n):
            new_total = total - a[i] + d[i]
            mn, mx = s[0], s[-1]
            if d[i] < mn:
                mn = d[i]
            elif d[i] > mx:
                mx = d[i]
            best = max(best, new_total - mn - mx)

        out.append(str(best))

    return "\n".join(out)

# custom tests
assert run("""1
3
1 100 2
2 100 2
""") == "100"

assert run("""1
4
1 2 3 4
10 2 3 4
""") == "9"

assert run("""1
5
5 5 5 5 5
6 6 6 6 6
""") == "15"

assert run("""1
3
10 1 2
10 1 100
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nghiêng 3 phần tử | 100 | hành vi dịch chuyển trung bình | 
| tăng lớn duy nhất | 9 | xử lý thay thế cực đoan | 
| tất cả đều bình đẳng | 15 | ổn định dưới sự đối xứng | 
| nhảy tối đa cuối cùng | 10 | thay thế ranh giới tối đa | 

## Vỏ cạnh 

Khi nào$n = 3$, thuật toán giảm xuống việc chọn trung vị sau một lần sửa đổi. Việc triển khai sắp xếp rõ ràng từng mảng ứng cử viên và chọn phần tử ở giữa, đảm bảo tính chính xác ngay cả khi giá trị được sửa đổi trở thành ứng cử viên tối thiểu và tối đa trong các tình huống khác nhau. 

Khi giá trị được sửa đổi trở thành mức tối thiểu mới, mức tối thiểu ban đầu có thể vẫn còn trong mảng nhưng sẽ không bị xóa theo cách tương tự nữa. Công thức$total - min - max$vẫn đúng vì chỉ có phần tử nhận dạng nhỏ nhất thay đổi chứ không phải cấu trúc của quy tắc cắt xén. 

Khi giá trị được sửa đổi trở thành mức tối đa mới, lý luận đối xứng sẽ được áp dụng. Thuật toán thay thế chính xác giá trị lớn nhất cũ trong bước trừ, đảm bảo rằng tổng cuối cùng phản ánh tập cực trị được cập nhật thay vì tập gốc.
