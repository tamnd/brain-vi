---
title: "CF 1382A - Dãy số chung"
description: "Chúng ta có hai mảng số nguyên và chúng ta muốn xây dựng mảng thứ ba có thể thu được bằng cách xóa các phần tử khỏi cả hai mảng. Nói cách khác, chúng ta đang tìm kiếm một chuỗi các giá trị xuất hiện trong cả hai mảng trong khi vẫn giữ nguyên thứ tự trong mỗi mảng."
date: "2026-06-16T13:58:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1382
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 658 (Div. 2)"
rating: 800
weight: 1382
solve_time_s: 401
verified: false
draft: false
---

[CF 1382A - Dãy số chung](https://codeforces.com/problemset/problem/1382/A) 

**Đánh giá:** 800 
**Tags:** vũ lực 
**Thời gian giải:** 6 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng số nguyên và chúng ta muốn xây dựng mảng thứ ba có thể thu được bằng cách xóa các phần tử khỏi cả hai mảng. Nói cách khác, chúng ta đang tìm kiếm một chuỗi các giá trị xuất hiện trong cả hai mảng trong khi vẫn giữ nguyên thứ tự trong mỗi mảng. 

Mục tiêu là xuất ra bất kỳ dãy con chung nào như vậy, nhưng với mức ưu tiên cao nhất là giảm thiểu độ dài của nó. Nếu không tồn tại dãy con chung thì chúng ta phải báo cáo thực tế đó. 

Nhận xét quan trọng từ định nghĩa về dãy con là bất kỳ câu trả lời hợp lệ nào cũng chỉ được bao gồm các giá trị xuất hiện trong cả hai mảng. Tuy nhiên, điều này thôi thì chưa đủ vì trật tự rất quan trọng. Một giá trị xuất hiện trong cả hai mảng không tự động đảm bảo rằng nó có thể tạo thành một dãy con có độ dài lớn hơn một dãy có các giá trị khác. 

Các ràng buộc đủ nhỏ để cả hai mảng cùng chứa tối đa khoảng hai nghìn phần tử trong tất cả các thử nghiệm. Điều này ngay lập tức gợi ý rằng một$O(nm)$so sánh giữa các mảng là an toàn, vì ngay cả trong trường hợp xấu nhất, chúng tôi thực hiện tổng cộng khoảng một triệu thao tác trên mỗi bộ thử nghiệm, nằm trong giới hạn thoải mái. 

Trường hợp cạnh tinh vi nhất phát sinh từ sự trùng lặp và thứ tự. Một cách tiếp cận ngây thơ chỉ đơn giản là thu thập tất cả các giá trị giao nhau mà không kiểm tra tính khả thi của chuỗi sau có thể dễ dàng thất bại. Ví dụ, hãy xem xét:```
a = [1, 2]
b = [2, 1]
```Giao điểm là {1, 2}, nhưng không phải`[1, 2]`cũng không`[2, 1]`xuất hiện dưới dạng một chuỗi con trong cả hai mảng. Câu trả lời đúng phải có độ dài 1 và`[1]`hoặc`[2]`là hợp lệ. Điều này cho thấy chúng ta không thể coi vấn đề như một giao điểm đã định sẵn. 

Một dạng lỗi khác là giả sử chúng ta cần xây dựng dãy con chung dài nhất. Điều đó là không cần thiết và phức tạp hơn nhiều; bài toán chỉ yêu cầu dãy con chung không trống ngắn nhất, làm thay đổi hoàn toàn cấu trúc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là liệt kê tất cả các chuỗi con của mảng đầu tiên và kiểm tra xem mỗi chuỗi có xuất hiện trong mảng thứ hai hay không. Điều này đúng vì nó khám phá rõ ràng không gian tìm kiếm của các ứng viên hợp lệ. Tuy nhiên, số lượng các chuỗi con là theo cấp số nhân, cụ thể là$2^n$, vì vậy ngay cả đối với$n = 1000$điều này trở nên hoàn toàn không khả thi. 

Một sự quan sát tập trung hơn sẽ thay đổi mọi thứ. Dãy con chung ngắn nhất có thể chỉ có thể có độ dài 1 hoặc 2 trong bất kỳ tình huống có ý nghĩa nào, bởi vì nếu tồn tại bất kỳ phần tử chung nào giữa các mảng thì chuỗi một phần tử đã là tối ưu. Không cần phải xem xét các chuỗi dài hơn trừ khi bị ràng buộc bởi các ràng buộc, nhưng ở đây giải pháp có độ dài 1 luôn tốt hơn bất cứ khi nào có thể. 

Vì vậy, nhiệm vụ thực sự là kiểm tra xem có giá trị nào xuất hiện trong cả hai mảng hay không. Nếu giá trị đó tồn tại, chúng tôi sẽ xuất giá trị đó ngay lập tức. Nếu không thì không có dãy con chung nào cả. 

Điều này làm giảm vấn đề từ tìm kiếm tổ hợp sang kiểm tra tần suất hoặc tư cách thành viên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các phần tiếp theo) |$O(2^n \cdot m)$|$O(1)$| Quá chậm | 
| Kiểm tra tần suất/ngã tư |$O(n + m)$|$O(1000)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành từng trường hợp kiểm thử. 

1. Xây dựng mảng tần số hoặc tập băm cho các phần tử trong mảng đầu tiên. Điều này cho phép kiểm tra tư cách thành viên liên tục sau này. 
2. Quét qua mảng thứ hai và kiểm tra xem có phần tử nào tồn tại trong tập hợp của mảng đầu tiên hay không. Phần tử đầu tiên được tìm thấy ngay lập tức tạo thành câu trả lời hợp lệ có độ dài 1. 
3. Nếu chúng tôi tìm thấy phần tử như vậy, chúng tôi sẽ xuất nó và ngừng xử lý trường hợp thử nghiệm đó. Lý do chúng ta có thể dừng ngay lập tức là vì độ dài 1 là độ dài tối thiểu có thể có đối với bất kỳ chuỗi không trống nào. 
4. Nếu chúng tôi quét xong mà không tìm thấy bất kỳ phần tử chung nào, chúng tôi sẽ đưa ra rằng không có giải pháp nào tồn tại. 

Tại sao nó hoạt động được đưa ra một lập luận tối thiểu. Bất kỳ dãy con chung hợp lệ nào cũng phải chứa ít nhất một giá trị xuất hiện trong cả hai mảng. Nếu một giá trị như vậy tồn tại thì chuỗi chỉ bao gồm giá trị đó đã là một chuỗi con hợp lệ của cả hai mảng. Vì vậy, bất kỳ chuỗi dài hơn nào cũng không thể cải thiện được tính tối ưu. Nếu không có giá trị nào được chia sẻ thì không có dãy chung nào khác trống có thể tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    s = set(a)
    ans = None
    
    for x in b:
        if x in s:
            ans = x
            break
    
    if ans is None:
        print("NO")
    else:
        print("YES")
        print(1, ans)
```Giải pháp này xây dựng một tập hợp từ mảng đầu tiên để hỗ trợ các truy vấn thành viên theo thời gian cố định. Sau đó, nó quét mảng thứ hai một lần và dừng ở giá trị được chia sẻ đầu tiên, đảm bảo đầu ra có độ dài tối thiểu. 

Một điểm tinh tế là chúng ta không cần xem xét việc đặt hàng ngoài tư cách thành viên. Vì một phần tử không có ràng buộc về thứ tự nên bất kỳ sự xuất hiện nào trong cả hai mảng đều đủ. Việc thoát sớm cũng rất quan trọng đối với hiệu quả nhưng không cần thiết đối với tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
a = [10, 8, 6, 4]
b = [1, 2, 3, 4, 5]
```| Bước | Phần tử b hiện tại | Trong tập hợp (a)? | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | Không | Không có | 
| 2 | 2 | Không | Không có | 
| 3 | 3 | Không | Không có | 
| 4 | 4 | Có | 4 | 

Chúng ta dừng ngay lập tức khi gặp số 4 ở mảng thứ hai vì nó có mặt ở mảng đầu tiên. Điều này xác nhận rằng giải pháp có độ dài 1 luôn được ưu tiên khi có sẵn. 

### Ví dụ 2```
a = [3]
b = [2]
```| Bước | Phần tử b hiện tại | Trong tập hợp (a)? | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 | Không | Không có | 

Chúng tôi kết thúc quá trình quét mà không tìm thấy bất kỳ kết quả trùng khớp nào, do đó không tồn tại chuỗi con chung nào. Điều này chứng tỏ trường hợp câu trả lời phải là “KHÔNG”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi mảng được xử lý một lần, với việc tra cứu được thiết lập theo thời gian không đổi | 
| Không gian |$O(n)$| Lưu trữ các phần tử của mảng đầu tiên trong tập băm | 

Tổng kích thước của tất cả các trường hợp thử nghiệm đều nhỏ, do đó giải pháp tuyến tính này dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ ngay cả trong trường hợp đầu vào xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        s = set(a)
        ans = None
        for x in b:
            if x in s:
                ans = x
                break
        
        if ans is None:
            out.append("NO")
        else:
            out.append("YES")
            out.append(f"1 {ans}")
    
    return "\n".join(out)

# sample 1
assert run("""5
4 5
10 8 6 4
1 2 3 4 5
1 1
3
3
1 1
3
2
5 3
1000 2 2 2 3
3 1 5
5 5
1 2 3 4 5
1 2 3 4 5
""") == """YES
1 4
YES
1 3
NO
YES
1 3
YES
1 2"""

# all disjoint
assert run("""1
3 3
1 2 3
4 5 6
""") == "NO"

# single overlap
assert run("""1
3 3
1 2 3
3 4 5
""") == "YES\n1 3"

# all same
assert run("""1
4 4
7 7 7 7
7 7 7 7
""") == "YES\n1 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng rời rạc | KHÔNG | không có trường hợp phần tử chung | 
| chồng chéo đơn | CÓ 1 x | trường hợp tích cực cơ bản | 
| mảng giống hệt nhau | CÓ 1 x | trùng lặp không thành vấn đề | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi mảng chứa các giá trị lặp lại nhưng không cần cấu trúc riêng biệt được chia sẻ. 

Ví dụ:```
a = [5, 5, 5]
b = [5]
```Thuật toán chèn 5 vào tập hợp từ mảng đầu tiên. Trong khi quét mảng thứ hai, nó ngay lập tức tìm thấy 5 và trả về`[5]`. Mặc dù bội số là khác nhau nhưng giá trị của dãy con chỉ phụ thuộc vào sự tồn tại chứ không phụ thuộc vào tần số. 

Một trường hợp khác là đảo ngược thứ tự:```
a = [1, 2]
b = [2, 1]
```Quá trình quét tìm thấy 2 hoặc 1 tùy thuộc vào thứ tự duyệt của`b`, nhưng cả hai đều là câu trả lời hợp lệ. Điều này xác nhận rằng thuật toán không dựa vào căn chỉnh hoặc khớp vị trí, chỉ dựa vào thành viên, điều này là đủ vì đầu ra bị ràng buộc ở độ dài 1.
