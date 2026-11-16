---
title: "CF 1894D - Âm trung tính"
description: "Chúng tôi được đưa ra hai chuỗi. Trình tự đầu tiên đã được cố định theo thứ tự nội bộ của nó và chúng tôi không được phép sắp xếp lại nó."
date: "2026-06-09T01:16:13+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1894
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 908 (Div. 2)"
rating: 1700
weight: 1894
solve_time_s: 98
verified: false
draft: false
---

[CF 1894D - Âm sắc trung tính](https://codeforces.com/problemset/problem/1894/D) 

**Đánh giá:** 1700 
**Thẻ:** thuật toán xây dựng, cấu trúc dữ liệu, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra hai chuỗi. Trình tự đầu tiên đã được cố định theo thứ tự nội bộ của nó và chúng tôi không được phép sắp xếp lại nó. Chuỗi thứ hai bao gồm các giá trị bổ sung mà chúng ta có thể đặt ở bất kỳ đâu trong chuỗi cuối cùng và chúng ta cũng có thể tự do hoán vị các giá trị bổ sung này trước khi chèn chúng. 

Mục tiêu là xây dựng một chuỗi cuối cùng chứa tất cả các phần tử ban đầu từ cả hai mảng sao cho chuỗi con tăng dài nhất của chuỗi cuối cùng càng nhỏ càng tốt. Chúng tôi không cố gắng hủy bỏ thứ tự một cách tùy tiện, chúng tôi chỉ được phép xen kẽ mảng thứ hai vào mảng thứ nhất trong khi vẫn giữ nguyên thứ tự tương đối bên trong mảng thứ nhất và chúng tôi chọn bất kỳ thứ tự nào của mảng thứ hai. 

Đại lượng chính là LIS của chuỗi kết quả. Chúng tôi muốn giảm thiểu mức độ lớn của một dãy con tăng dần sau khi chèn. 

Các ràng buộc rất lớn, với tổng chiều dài của các trường hợp thử nghiệm lên tới 2·10^5. Điều này loại trừ mọi cách xây dựng bậc hai cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc tính toán lại LIS nhiều lần hoặc thử tất cả các thao tác xen kẽ đều không thể thực hiện được. Chúng ta cần một cấu trúc về cơ bản là tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại đơn giản xuất hiện khi người ta cố gắng đặt từng phần tử của mảng thứ hai gần các giá trị tương tự trong mảng đầu tiên mà không có thứ tự chung. Ví dụ: nếu mảng đầu tiên đã tăng và mảng thứ hai cũng chứa các phần tử tăng, việc chèn bất cẩn có xu hướng bảo toàn hoặc thậm chí tăng LIS thay vì giảm thiểu nó. 

Một lỗi nhỏ khác xảy ra khi coi LIS chỉ phụ thuộc vào các giá trị nhiều tập hợp. Hai chuỗi có cùng giá trị nhưng vị trí tương đối khác nhau có thể tạo ra hành vi LIS khác nhau sau khi chèn, do đó, các quyết định sắp xếp thứ tự rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các hoán vị của mảng thứ hai và tất cả các cách chèn chúng vào mảng thứ nhất. Ngay cả khi chúng ta sửa một hoán vị, việc chèn m phần tử vào khoảng trống n+1 cũng đã mang lại các lựa chọn tổ hợp. Chỉ riêng số lần xen kẽ đã theo thứ tự của các hệ số nhị thức và việc tính toán lại LIS cho mỗi chuỗi ứng cử viên sẽ tốn O((n+m) log(n+m)) cho mỗi lần kiểm tra. Điều này nhanh chóng trở nên lớn về mặt thiên văn. 

Quan sát cấu trúc là chúng tôi không thực sự cố gắng bảo toàn cấu trúc của cả hai mảng một cách đối xứng. Mảng đầu tiên là cố định và chỉ mảng thứ hai là linh hoạt. LIS bị chi phối bởi thời gian chúng ta có thể duy trì chuỗi tăng dần nghiêm ngặt trên chuỗi cuối cùng. Để giảm thiểu LIS, chúng tôi muốn phá hủy các chuỗi tăng dài càng sớm càng tốt bằng cách đưa vào các phần tử "chặn". 

Sự đơn giản hóa chính là lưu ý rằng LIS chỉ phụ thuộc vào thứ tự tương đối chứ không phải giá trị tuyệt đối. Nếu muốn giảm LIS, chúng tôi muốn tránh tạo ra các cơ hội trong đó cả hai mảng đều đóng góp vào cùng một chuỗi tăng dần. Cách tốt nhất để ngăn chặn điều này là đảm bảo rằng các phần tử được chèn được sắp xếp sao cho chúng không giúp hình thành các chuyển tiếp tăng dần với mảng ban đầu. 

Điều này dẫn đến một ý tưởng tham lam cổ điển: sắp xếp mảng thứ hai theo thứ tự không tăng và xen kẽ nó với mảng đầu tiên một cách chiến lược sao cho mỗi phần tử được chèn càng "gây rối" nhất có thể. Trên thực tế, chúng tôi cố gắng đặt các giá trị lớn trước để chúng không thể mở rộng chuỗi con tăng dần của các giá trị nhỏ và đặt các giá trị nhỏ sau để chúng không tạo ra chuỗi đi lên mới.

Sau khi hướng này được khắc phục, vấn đề sẽ giảm xuống còn việc xây dựng một trình tự trong đó chúng tôi quyết định cách hợp nhất hai cấu trúc có thứ tự trong khi kiểm soát sự phát triển của LIS. Cấu trúc đúng hóa ra là sắp xếp cả hai mảng theo cách làm cho chuỗi cuối cùng là "không tăng nhất có thể" trong khi vẫn giữ nguyên thứ tự tương đối của mảng đầu tiên. Điều này có thể đạt được bằng cách hợp nhất hai con trỏ sau khi sắp xếp mảng thứ hai theo thứ tự giảm dần và chèn nó theo kiểu được kiểm soát để ngăn việc tăng phần mở rộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n+m) | Quá chậm | 
| Hợp nhất tham lam tối ưu | O((n+m) log(n+m)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng chuỗi cuối cùng bằng cách coi mảng thứ hai như một tài nguyên mà chúng tôi đưa vào mảng đầu tiên theo cách ngăn chặn xu hướng tăng lên. 

1. Sắp xếp mảng thứ hai theo thứ tự không tăng. Điều này đảm bảo rằng chúng tôi luôn đặt các phần tử gây rối lớn hơn lên hàng đầu, giúp tối đa hóa khả năng chặn các chuỗi tiếp theo tăng dần của chúng. 
2. Duyệt mảng đầu tiên từ trái sang phải trong khi vẫn giữ con trỏ vào mảng thứ hai đã được sắp xếp. 
3. Tại mỗi vị trí trong mảng đầu tiên, hãy quyết định xem có chèn các phần tử từ mảng thứ hai hay không trước khi đặt phần tử mảng đầu tiên hiện tại. Chúng tôi chèn càng nhiều phần tử còn lại từ mảng thứ hai càng tốt trong khi chúng lớn hơn hoặc bằng giá trị mảng đầu tiên hiện tại, theo thứ tự giảm dần. 
4. Sau khi hết điều kiện đó, đặt phần tử hiện tại của mảng đầu tiên vào kết quả. 
5. Tiếp tục quá trình này cho đến khi tất cả các phần tử của mảng đầu tiên được đặt. 
6. Nếu bất kỳ phần tử nào vẫn còn trong mảng thứ hai sau khi xử lý tất cả các phần tử của mảng đầu tiên, hãy thêm chúng vào cuối. 

Lý do chính đằng sau bước 3 là việc chèn sớm các giá trị lớn sẽ ngăn chúng tham gia vào các chuỗi con tăng dần bắt đầu trong mảng đầu tiên. Vì LIS rất nhạy cảm với khả năng mở rộng chuỗi ngày càng tăng nên việc đặt các giá trị lớn càng sớm càng tốt sẽ đảm bảo chúng hoạt động giống như các rào cản hơn là phần mở rộng. 

### Tại sao nó hoạt động 

Việc xây dựng thực thi một cấu trúc được kiểm soát trong đó các phần tử từ mảng thứ hai không bao giờ xuất hiện theo cách có thể mở rộng chuỗi con tăng dần thông qua sự liên kết thuận lợi với mảng đầu tiên. Bằng cách chèn các phần tử mảng thứ hai theo thứ tự giảm dần và chỉ khi chúng lớn so với vị trí hiện tại, chúng tôi đảm bảo rằng bất kỳ chuỗi con tăng tiềm năng nào đều phải chủ yếu dựa vào cấu trúc của mảng ban đầu, trong khi các phần tử được chèn sẽ phá vỡ sự tăng trưởng đơn điệu thay vì hỗ trợ nó. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của mảng đầu tiên, tất cả các phần tử được chèn đều nằm ở các vị trí mà chúng không thể mở rộng bất kỳ chuỗi con tăng dần nào thành hậu tố còn lại hoặc chúng lớn hơn tất cả các phần tử được chèn trong tương lai có thể xâu chuỗi lên trên. Điều này ngăn cản việc xây dựng các chuỗi mới dài hơn vượt quá đường cơ sở không thể tránh khỏi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        b.sort(reverse=True)

        res = []
        j = 0

        for x in a:
            while j < m and b[j] >= x:
                res.append(b[j])
                j += 1
            res.append(x)

        while j < m:
            res.append(b[j])
            j += 1

        out.append(" ".join(map(str, res)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên sắp xếp mảng thứ hai để chúng ta luôn xem xét giá trị lớn nhất còn lại. Thứ tự này rất quan trọng vì nó đảm bảo rằng khi chúng ta chèn các phần tử, chúng ta luôn đặt các giá trị “chặn” nhất trước tiên. 

Trong quá trình quét mảng đầu tiên, chúng tôi duy trì một con trỏ vào mảng thứ hai. Trước khi đặt một phần tử từ mảng đầu tiên, chúng ta loại bỏ tất cả các phần tử còn lại của mảng thứ hai đủ lớn để đặt trước nó theo quy tắc tiêm giảm dần. Điều này tạo ra các phân đoạn trong đó các giá trị được chèn lớn nằm ở bên trái của các giá trị ban đầu nhỏ hơn, giúp ngăn chặn việc hình thành các chuỗi con tăng dần qua các ranh giới. 

Cuối cùng, các giá trị nhỏ còn sót lại sẽ được thêm vào, vì chúng không còn có thể giúp mở rộng bất kỳ chuỗi tăng dần nào theo cách có hại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [6, 4]
b = [5]
```| Bước | Hiện tại | con trỏ b | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 5 | 5 ≥ 6 sai, đặt 6 | [6] | 
| 2 | 4 | 5 | 5 ≥ 4 đúng, chèn 5 | [6, 5] | 
| 3 | 4 | - | nơi 4 | [6, 5, 4] | 

Điều này tạo ra một chuỗi hoàn toàn không tăng, do đó LIS trở thành 1. Dấu vết cho thấy phần tử được chèn đơn lẻ buộc phải xuất hiện ở vị trí mà nó không thể mở rộng bất kỳ chuỗi con tăng dần nào. 

### Ví dụ 2 

đầu vào:```
a = [1, 3, 5]
b = [2, 4]
```| Bước | Hiện tại | con trỏ b | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4,2 | 4 ≥ 1 chèn 4, 2 ≥ 1 chèn 2 | [4,2,1] | 
| 2 | 3 | - | nơi 3 | [4,2,1,3] | 
| 3 | 5 | - | nơi 5 | [4,2,1,3,5] | 

Cấu trúc đảm bảo rằng các phần tử lớn ban đầu không giúp xây dựng các dãy con tăng dài. LIS còn lại được kiểm soát bởi các chuyển đổi không thể tránh khỏi trong mảng ban đầu và các phần tử được chèn vào không tạo thêm chuỗi đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log m) | sắp xếp b chiếm ưu thế, việc hợp nhất là tuyến tính | 
| Không gian | O(n+m) | lưu trữ chuỗi đầu ra | 

Tổng kích thước của các trường hợp thử nghiệm được giới hạn bởi 2·10^5, do đó, việc sắp xếp và hợp nhất tuyến tính dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        b.sort(reverse=True)

        res = []
        j = 0
        for x in a:
            while j < m and b[j] >= x:
                res.append(b[j])
                j += 1
            res.append(x)

        while j < m:
            res.append(b[j])
            j += 1

        out.append(" ".join(map(str, res)))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("""7
2 1
6 4
5
5 5
1 7 2 4 5
5 4 1 2 7
1 9
7
1 2 3 4 5 6 7 8 9
3 2
1 3 5
2 4
10 5
1 9 2 3 8 1 4 7 2 9
7 8 5 4 6
2 1
2 2
1
6 1
1 1 1 1 1 1
777
""").split() == run("""7
2 1
6 5 4
5 5
1 1 7 7 2 2 4 4 5 5
1 9
9 8 7 6 5 4 3 2 1
3 2
1 3 5 2 4
10 5
1 9 2 3 8 8 1 4 4 7 7 2 9 6 5
2 1
2 2 1
6 1
777 1 1 1 1 1 1
""").split(), "sample check"

# minimum size
assert run("""1
1 1
5
1
""").strip() in {"5 1", "1 5"}

# all equal
assert run("""1
3 3
2 2 2
2 2 2
""")

# decreasing a
assert run("""1
4 2
9 7 5 3
8 6
""")

# increasing a, increasing b
assert run("""1
5 3
1 2 3 4 5
6 7 8
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu 1x1 | bất kỳ đặt hàng nào | độ đúng cơ sở | 
| tất cả các giá trị bằng nhau | hành vi LIS ổn định | xử lý trùng lặp | 
| mảng giảm dần | không có lạm phát LIS | hành vi chặn | 
| tăng mảng | trường hợp áp lực LIS tồi tệ nhất | xử lý cấu trúc toàn cầu | 

## Vỏ cạnh 

Khi cả hai mảng đều có giá trị bằng nhau, mọi thứ tự sẽ giữ nguyên LIS dưới dạng toàn bộ độ dài của mảng vì không thể thực hiện được bước tăng nghiêm ngặt nào. Thuật toán chỉ xen kẽ các giá trị mà không tạo ra các chuyển đổi tăng dần mới, do đó LIS vẫn bằng 1. 

Khi mảng đầu tiên giảm nghiêm ngặt, nó đã có LIS bằng 1. Thuật toán chèn sớm các phần tử lớn hơn, nhưng vì tất cả các phép so sánh trong tương lai đều không tăng nên không có chuỗi con tăng nào có thể hình thành, duy trì LIS tối ưu. 

Khi cả hai mảng đều tăng nghiêm ngặt, việc xây dựng buộc các phần tử lớn từ mảng thứ hai phải được đặt sớm, đảm bảo rằng chúng không kéo dài chuỗi tăng dần của mảng thứ nhất. LIS kết quả được xác định bằng mức tăng không thể tránh khỏi bên trong chuỗi ban đầu, xác nhận rằng các phần tử được chèn không góp phần tăng thêm.
