---
title: "CF 2091B - Huấn luyện đội ngũ"
description: "Chúng ta được cấp một nhóm học sinh, mỗi học sinh có một giá trị kỹ năng cố định và chúng ta phải chia họ thành các đội sao cho mỗi học sinh thuộc đúng một đội. Đối với bất kỳ đội nào, sức mạnh của đội đó được xác định bằng số học sinh trong đội nhân với kỹ năng nhỏ nhất trong đội đó."
date: "2026-06-08T05:45:28+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2091
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1013 (Div. 3)"
rating: 800
weight: 2091
solve_time_s: 72
verified: true
draft: false
---

[CF 2091B - Đào tạo nhóm](https://codeforces.com/problemset/problem/2091/B) 

**Đánh giá:** 800 
**Tags:** dp, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một nhóm học sinh, mỗi học sinh có một giá trị kỹ năng cố định và chúng ta phải chia họ thành các đội sao cho mỗi học sinh thuộc đúng một đội. Đối với bất kỳ đội nào, sức mạnh của đội đó được xác định bằng số học sinh trong đội nhân với kỹ năng nhỏ nhất trong đội đó. Một đội chỉ được coi là hợp lệ nếu sức mạnh tính toán này đạt ít nhất một giá trị ngưỡng nhất định. 

Nhiệm vụ không phải là tối đa hóa sức mạnh tổng hợp hay cân bằng các đội mà chỉ đơn giản là tối đa hóa số lượng đội thỏa mãn điều kiện sức mạnh sau khi chia mỗi người đúng một lần. 

Điểm căng thẳng chính là việc phân nhóm nhiều học sinh hơn sẽ làm tăng hệ số nhân, nhưng nó có thể làm giảm kỹ năng tối thiểu trong nhóm. Cấu trúc của nhóm quyết định đầy đủ liệu một đội có đủ điều kiện hay không. 

Từ những ràng buộc, tổng số học sinh trong tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng xem xét tất cả các phân vùng hoặc thậm chí tất cả các tập hợp con, vì chúng tăng theo cấp số nhân. Ngay cả O(n^2) cho mỗi trường hợp thử nghiệm cũng không an toàn trong trường hợp tổng hợp xấu nhất. Chúng ta nên mong đợi một cách tiếp cận tham lam tuyến tính hoặc gần tuyến tính dựa trên sắp xếp. 

Một trường hợp tế nhị phát sinh khi một học sinh có kỹ năng cực cao nhưng lại bị bỏ lại một mình. Ví dụ: nếu x lớn và tất cả các kỹ năng đều nhỏ ngoại trừ một giá trị lớn, thì một kẻ tham lam ngây thơ có thể cô lập những học sinh giỏi một cách không chính xác hoặc hợp nhất quá mạnh mẽ. Một trường hợp thất bại khác là khi việc phân nhóm được thực hiện mà không cân nhắc rằng việc thêm một học sinh yếu có thể làm giảm mức tối thiểu và phá vỡ một đội hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ thử mọi cách có thể để chia học sinh thành các đội và kiểm tra xem có bao nhiêu đội thỏa mãn điều kiện. Ngay cả việc hạn chế chúng ta xây dựng các nhóm một cách tham lam theo các trật tự khác nhau vẫn để lại sự bùng nổ tổ hợp trong việc phân nhóm các lựa chọn. Với n sinh viên, số lượng phân vùng là theo cấp số nhân và thậm chí việc quyết định hợp nhất tối ưu cục bộ là không đủ vì các quyết định sớm ảnh hưởng đến tính khả thi sau này. 

Quan sát quan trọng đến từ việc viết lại điều kiện cho một đội. Nếu đội có kỹ năng tối thiểu m và cỡ k thì mạnh khi k · m ≥ x, hoặc tương đương k ≥ ceil(x/m). Điều này có nghĩa là khi chúng tôi khắc phục được thành viên yếu nhất trong nhóm, chúng tôi sẽ biết số lượng người tối thiểu cần thiết để hỗ trợ họ. 

Điều này gợi ý việc sắp xếp học sinh theo kỹ năng và cố gắng “phân công hỗ trợ” cho những học sinh yếu hơn bằng cách sử dụng những học sinh giỏi hơn. Một cách hữu ích để suy nghĩ về nó là xử lý học sinh từ mạnh nhất đến yếu nhất, tích lũy một nhóm ứng cử viên. Khi quy mô tích lũy đủ để hỗ trợ mức tối thiểu hiện tại, chúng tôi có thể hoàn thiện một nhóm ngay lập tức. Vì chúng tôi đang xử lý theo thứ tự giảm dần nên học sinh hiện tại luôn là học sinh nhỏ nhất trong nhóm hiện tại. 

Sự tham lam này có tác dụng vì mỗi khi chúng tôi quyết định thành lập một đội, việc sử dụng những học sinh mạnh nhất hiện có trước tiên sẽ đảm bảo rằng chúng tôi không lãng phí sức mạnh tiềm tàng cho những nhóm yếu hơn trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân loại tham lam | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng kỹ năng theo thứ tự giảm dần. Điều này đảm bảo rằng khi chúng ta xây dựng đội ngũ dần dần, yếu tố được thêm vào cuối cùng luôn là yếu tố yếu nhất trong phân khúc hiện tại. Điều này giúp chúng ta dễ dàng đánh giá sức mạnh khi quyết định đóng cửa một đội. 
2. Duy trì bộ đếm`cnt`đại diện cho số lượng sinh viên hiện đang ở trong đội thành lập. Chúng tôi cũng duy trì số lượng đội hợp lệ mà chúng tôi đã thành lập. 
3. Lặp lại các kỹ năng đã được sắp xếp. Đối với mỗi học sinh, tăng dần`cnt`bởi vì chúng tôi đang tạm thời thêm họ vào đội ngũ hiện tại. 
4. Sau mỗi lần thêm, kiểm tra xem`cnt * current_skill ≥ x`. Từ`current_skill`là nhỏ nhất trong nhóm hiện tại (do sắp xếp), điều kiện này kiểm tra xem nhóm hiện tại có thể thành lập một nhóm hợp lệ hay không. 
5. Nếu điều kiện được thỏa mãn, chúng ta chốt nhóm này: tăng câu trả lời và đặt lại`cnt`về 0, bắt đầu một đội mới từ học sinh tiếp theo. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến rằng nhóm hiện tại luôn gồm những học sinh mạnh nhất còn lại theo thứ tự giảm dần nên phần tử được thêm vào cuối cùng luôn là kỹ năng tối thiểu trong nhóm đó. Bất cứ khi nào chúng tôi quyết định thành lập một nhóm, chúng tôi sẽ làm như vậy vào thời điểm sớm nhất có thể khi nhóm có hiệu lực. Việc trì hoãn sẽ chỉ bao gồm các thành phần yếu hơn trong các đội trong tương lai hoặc làm giảm số lượng các nhóm hợp lệ có thể có. Bởi vì chúng tôi luôn hoàn thiện ngay khi điều kiện được thỏa mãn nên chúng tôi không bao giờ bỏ lỡ một đội hình tiềm năng và chúng tôi không bao giờ lãng phí những học sinh giỏi bằng cách ghép họ thành những nhóm lớn không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        cnt = 0
        ans = 0
        
        for v in a:
            cnt += 1
            if cnt * v >= x:
                ans += 1
                cnt = 0
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc sắp xếp các kỹ năng theo thứ tự giảm dần để khi chúng ta xây dựng một nhóm một cách tuần tự thì thành phần hiện tại luôn là yếu nhất trong nhóm một phần đó. quầy`cnt`theo dõi quy mô của nhóm đang diễn ra. Mỗi lần mở rộng nhóm, chúng tôi sẽ kiểm tra xem việc nhân kích thước của nó với phần tử yếu nhất có thỏa mãn ngưỡng hay không. Nếu đúng như vậy, chúng tôi sẽ đóng nhóm ngay lập tức và thiết lập lại. 

Một sai lầm phổ biến là cố gắng duy trì nhiều nhóm được hình thành một phần hoặc cố gắng xếp học sinh vào các thùng mà không phân loại một cách tham lam. Thứ tự giảm dần đảm bảo rằng mỗi séc thể hiện chính xác mức tối thiểu thực sự của nhóm hiện tại. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi thuật toán trên hai đầu vào. 

Đầu vào đầu tiên:```
n = 6, x = 4
a = [4, 5, 3, 3, 2, 6]
```Sắp xếp giảm dần:```
[6, 5, 4, 3, 3, 2]
```| bước | giá trị | cnt | giá trị cnt * | hành động | đội | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | 1 | 6 | đội hình | 1 | 
| 2 | 5 | 1 | 5 | đội hình | 2 | 
| 3 | 4 | 1 | 4 | đội hình | 3 | 
| 4 | 3 | 1 | 3 | không | 3 | 
| 5 | 3 | 2 | 6 | đội hình | 4 | 
| 6 | 2 | 1 | 2 | không | 4 | 

Điều này cho thấy các phần tử mạnh có thể hình thành các nhóm đơn lẻ một cách nhanh chóng như thế nào, trong khi các phần tử yếu hơn có thể cần được nhóm lại. 

Đầu vào thứ hai:```
n = 5, x = 3
a = [5, 3, 2, 3, 2]
```Đã sắp xếp:```
[5, 3, 3, 2, 2]
```| bước | giá trị | cnt | giá trị cnt * | hành động | đội | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | đội hình | 1 | 
| 2 | 3 | 1 | 3 | đội hình | 2 | 
| 3 | 3 | 1 | 3 | đội hình | 3 | 
| 4 | 2 | 1 | 2 | không | 3 | 
| 5 | 2 | 2 | 4 | đội hình | 4 | 

Dấu vết cho thấy các phần tử yếu hơn vẫn có thể hoàn thành các đội hợp lệ khi được tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp thống trị từng trường hợp thử nghiệm | 
| Không gian | O(1) thêm | sắp xếp tại chỗ và bộ đếm hằng số | 

Tổng số n trên các trường hợp thử nghiệm tối đa là 200.000, do đó cách tiếp cận O(n log n) đủ hiệu quả trong vòng 2 giây. Thuật toán chỉ thực hiện sắp xếp và quét tuyến tính một lần cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        cnt = 0
        ans = 0
        
        for v in a:
            cnt += 1
            if cnt * v >= x:
                ans += 1
                cnt = 0
        
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""5
6 4
4 5 3 3 2 6
4 10
4 2 1 3
5 3
5 3 2 3 2
3 6
9 1 7
6 10
6 1 3 6 3 2
""") == """4
0
4
2
1"""

# all equal values
assert run("""1
5 10
2 2 2 2 2
""") == "1"

# single strong element
assert run("""1
3 5
1 1 10
""") == "1"

# no valid teams
assert run("""1
4 100
1 2 3 4
""") == "0"

# minimum size
assert run("""1
1 1
1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 | sự ổn định nhóm | 
| yếu tố mạnh mẽ duy nhất | 1 | xử lý đúng các giá trị lớn bị cô lập | 
| không có đội hợp lệ | 0 | trường hợp thất bại khi không có gì thỏa mãn ngưỡng | 
| kích thước tối thiểu | 1 | trường hợp cạnh cơ sở | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các giá trị đều bằng nhau nhưng chỉ dưới ngưỡng khi nhân với kích thước đầy đủ. Ví dụ:```
n = 5, x = 10, a = [2,2,2,2,2]
```Sau khi sắp xếp, chúng ta tích lũy các số đếm: 1 cho 2, 2 cho 4, 3 cho 6, 4 cho 8, 5 cho 10, thế là thành lập một đội đầy đủ. Thuật toán trì hoãn việc hình thành một cách chính xác cho đến khi nhóm đủ lớn. 

Một trường hợp khác là khi một giá trị lớn duy nhất chiếm ưu thế:```
n = 3, x = 5, a = [10,1,1]
```Chúng tôi thành lập một nhóm ngay lập tức với 10, vì 1·10 ≥ 5 đã thỏa mãn ở kích thước 1. Các phần tử yếu còn lại có thể không thành lập được đội thứ hai, điều này cũng đúng. 

Trường hợp thứ ba là khi sự cám dỗ tham lam gợi ý nên ghép những yếu tố yếu trước. Việc sắp xếp ngăn ngừa lỗi này vì các phần tử yếu luôn xuất hiện muộn hơn, đảm bảo chúng chỉ đóng góp khi cần thiết thay vì tiêu thụ các phần tử mạnh sớm.
