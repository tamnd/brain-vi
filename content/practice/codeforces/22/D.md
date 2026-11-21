---
title: "CF 22D - Phân đoạn"
description: "Chúng ta có một số khoảng đóng trên trục số. Một chiếc đinh được đặt ở một tọa độ nguyên bao phủ mọi đoạn chứa tọa độ đó, bao gồm cả các điểm cuối."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 22
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 22 (Div. 2 Only)"
rating: 1900
weight: 22
solve_time_s: 85
verified: true
draft: false
---
[CF 22D - Phân đoạn](https://codeforces.com/problemset/problem/22/D) 

**Xếp hạng:** 1900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số khoảng đóng trên trục số. Một chiếc đinh được đặt ở một tọa độ nguyên bao phủ mọi đoạn chứa tọa độ đó, bao gồm cả các điểm cuối. Nhiệm vụ là chọn càng ít vị trí đinh càng tốt sao cho mỗi đoạn đều chứa ít nhất một điểm đã chọn. 

Một cách khác để suy nghĩ về vấn đề này là: chúng ta muốn tập hợp các điểm nguyên tối thiểu sao cho mọi khoảng đều cắt tập hợp đó. 

Các ràng buộc đủ nhỏ để nhiều cách tiếp cận có thể vượt qua. Có tối đa 1000 phân đoạn và tọa độ được giới hạn bởi 10000 về giá trị tuyệt đối. Một giải pháp bậc hai sẽ được chấp nhận, vì$10^6$hoạt động rất nhỏ trong giới hạn 1 giây. Thậm chí$O(n^3)$có thể sẽ tồn tại trong thực tế. Tuy nhiên, cấu trúc của bài toán thừa nhận một giải pháp tham lam cổ điển với$O(n \log n)$sự phức tạp. 

Một điểm tinh tế là các khoảng được đóng lại. Nếu một đoạn kết thúc ở vị trí 2 và một đoạn khác bắt đầu ở vị trí 2 thì một chiếc đinh ở vị trí 2 sẽ bao phủ cả hai. 

Hãy xem xét ví dụ này:```
2
0 2
2 5
```Câu trả lời đúng là một chiếc đinh ở vị trí 2. Việc thực hiện bất cẩn coi các khoảng thời gian là mở hoặc quên bao gồm các điểm cuối sẽ sử dụng sai hai chiếc đinh. 

Một trường hợp cạnh khác là các khoảng suy biến, trong đó cả hai điểm cuối đều bằng nhau.```
3
1 1
1 3
5 5
```Câu trả lời tối ưu sử dụng đinh ở 1 và 5. Giải pháp giả sử các khoảng luôn có độ dài dương có thể vô tình bỏ qua các đoạn điểm. 

Cũng có khả năng các điểm cuối đầu vào không được sắp xếp theo thứ tự.```
2
5 1
2 4
```Đoạn đầu tiên thực sự có ý nghĩa$[1,5]$. Việc quên bình thường hóa các điểm cuối có thể phá vỡ hoàn toàn việc phân loại và kiểm tra mức độ phù hợp. 

Chuỗi chồng chéo là một nguồn sai lầm khác.```
4
1 4
2 5
3 6
7 8
```Tất cả ba khoảng đầu tiên có thể được đóng bằng một chiếc đinh, nhưng không nhất thiết phải ở điểm cuối bên trái của khoảng đầu tiên. Tham lam lựa chọn sai hướng có thể tạo ra quá nhiều đinh. 

## Phương pháp tiếp cận 

Tư duy vũ phu bắt đầu bằng việc quan sát rằng tọa độ rất nhỏ. Vì mọi vị trí đinh hữu ích phải nằm đâu đó trên đường nguyên giữa$-10000$Và$10000$, chúng ta có thể thử mọi tọa độ và liên tục chọn các vị trí bao phủ nhiều khoảng chưa được khám phá. 

Vấn đề là điều này trở thành một tìm kiếm kiểu bìa cố định nếu chúng ta muốn mức tối thiểu thực sự. Việc kiểm tra toàn diện các tập hợp con tọa độ là theo cấp số nhân và nhanh chóng là không thể. Ngay cả việc giới hạn chúng tôi ở các điểm cuối theo khoảng thời gian vẫn có tới 2000 vị trí ứng viên và$2^{2000}$là vô vọng. 

Một cách tiếp cận thực tế hơn là lập trình động trên tọa độ. Đối với mỗi điểm nguyên, chúng tôi có thể theo dõi khoảng thời gian nào đã được bao phủ. Nhưng việc biểu diễn các tập con khoảng đòi hỏi$2^n$trạng thái, một lần nữa bùng nổ ngay cả đối với$n=1000$. 

Lý do khiến vấn đề trở nên dễ dàng hơn nhiều là do cấu trúc các khoảng trên một đường thẳng. Các khoảng thời gian có một thứ tự tự nhiên và một khi chúng ta đã xác định được vị trí đóng đinh, mọi khoảng thời gian kết thúc trước điểm đó sẽ được xử lý vĩnh viễn. Điều này cho phép một chiến lược tham lam. 

Điểm mấu chốt là: khi chúng ta quyết định đóng một chiếc đinh, việc đặt nó càng xa càng tốt luôn là điều tối ưu. Giả sử chúng ta xét đoạn có điểm cuối bên phải nhỏ nhất. Bất kỳ giải pháp hợp lệ nào cũng phải đặt một số đinh bên trong đoạn này. Nếu chúng ta đặt chiếc đinh chính xác tại điểm cuối bên phải của đoạn đó, chúng ta sẽ tối đa hóa khả năng chiếc đinh này cũng bao phủ các khoảng thời gian trong tương lai. 

Quan sát đó dẫn trực tiếp đến thuật toán tham lam đâm theo khoảng thời gian cổ điển: 

1. Sắp xếp các đoạn theo điểm cuối bên phải. 
2. Lấy đoạn không che đầu tiên. 
3. Đặt đinh vào điểm cuối bên phải của nó. 
4. Xóa mọi đoạn chứa điểm đó. 
5. Lặp lại. 

Sự lựa chọn tham lam là an toàn vì bất kỳ giải pháp nào cũng cần có một cái đinh bên trong phân khúc kết thúc sớm nhất hiện tại. Di chuyển chiếc đinh đó đến điểm cuối bên phải của đoạn không thể làm giảm phạm vi bao phủ của các khoảng thời gian trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phân đoạn và chuẩn hóa từng phân đoạn sao cho$l \le r$. 

Điều này ngăn chặn sự so sánh không chính xác khi các điểm cuối được đưa ra theo thứ tự ngược lại. 
2. Sắp xếp các đoạn theo điểm cuối bên phải tăng dần. 

Đoạn kết thúc sớm nhất là đoạn hạn chế nhất. Nếu chúng ta trì hoãn việc xử lý nó, chúng ta có nguy cơ bỏ lỡ cơ hội hoàn thiện nó cùng với các phân đoạn sau. 
3. Khởi tạo một danh sách trống các vị trí đinh. 
4. Lặp lại các phân đoạn được sắp xếp từ trái sang phải. 
5. Đối với phân khúc hiện tại$[l,r]$, hãy kiểm tra xem chiếc đinh được đặt gần đây nhất đã nằm bên trong nó chưa. 

Vì các phân đoạn được xử lý bằng cách tăng điểm cuối bên phải nên chỉ những chiếc đinh mới nhất mới quan trọng. Những móng tay trước đó thậm chí còn ở xa hơn. 
6. Nếu phân đoạn hiện tại đã được đề cập, hãy tiếp tục đến phân đoạn tiếp theo. 
7. Nếu không, hãy đặt một chiếc đinh mới vào vị trí$r$, điểm cuối bên phải của đoạn. 

Đây là bước tham lam. Việc chọn điểm hợp lệ xa nhất có thể sẽ mang lại sự trùng lặp tối đa với các phân đoạn trong tương lai. 
8. Thêm chiếc đinh này vào danh sách câu trả lời. 
9. Sau khi xử lý tất cả các phân đoạn, xuất ra số lượng đinh và tọa độ của chúng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên$k$các khoảng được sắp xếp, thuật toán đã sử dụng số lượng đinh tối thiểu có thể để che chúng. 

Lấy khoảng không được khám phá đầu tiên theo thứ tự được sắp xếp, gọi nó là$[l,r]$. Bất kỳ giải pháp hợp lệ nào cũng phải đặt một số đinh vào trong khoảng này. Giả sử phương án tối ưu sử dụng một chiếc đinh ở vị trí$x$, Ở đâu$l \le x \le r$. Thay cái đinh đó bằng cái đinh lúc$r$không thể ảnh hưởng đến phạm vi bảo hiểm trong tương lai, bởi vì mọi khoảng thời gian trong tương lai đều kết thúc không sớm hơn$r$. Bất kỳ khoảng nào chứa$x$và xuất hiện sau đó cũng phải kéo dài ít nhất$r$, vậy nó vẫn chứa$r$. 

Điều đó có nghĩa là luôn tồn tại một giải pháp tối ưu phù hợp với sự lựa chọn tham lam. Sau khi sửa chiếc đinh đó, bài toán con còn lại có cấu trúc giống hệt nên việc lặp lại luận cứ chứng tỏ tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    segments = []
    
    for _ in range(n):
        l, r = map(int, input().split())
        
        if l > r:
            l, r = r, l
        
        segments.append((l, r))
    
    segments.sort(key=lambda x: x[1])
    
    nails = []
    
    for l, r in segments:
        if nails and l <= nails[-1] <= r:
            continue
        
        nails.append(r)
    
    print(len(nails))
    print(*nails)

solve()
```Phần đầu tiên đọc và bình thường hóa các khoảng thời gian. Hoán đổi điểm cuối khi$l > r$tránh các lỗi tinh vi sau này trong quá trình phân loại và kiểm tra ngăn chặn. 

Thứ tự sắp xếp là trái tim của chiến lược tham lam. Bằng cách xử lý các khoảng thời gian có điểm cuối bên phải nhỏ hơn trước, chúng tôi đảm bảo rằng mọi quyết định đều được đưa ra càng sớm càng tốt. 

các`nails`danh sách lưu trữ tất cả các vị trí đã chọn. Thuật toán chỉ kiểm tra móng gần đây nhất theo thứ tự sắp xếp. Những chiếc đinh trước luôn nhỏ hơn hoặc bằng chiếc đinh mới nhất, vì vậy nếu chiếc đinh mới nhất không bao phủ khoảng thời gian hiện tại thì những chiếc đinh cũ hơn cũng không thể làm được. 

Điều kiện:```
if nails and l <= nails[-1] <= r:
```kiểm tra xem khoảng thời gian hiện tại đã được bao phủ chưa. Vì các khoảng được đóng lại nên sự so sánh bao gồm sự bình đẳng ở cả hai phía. 

Khi phạm vi bảo hiểm không thành công, chúng tôi sẽ nối thêm`r`, điểm cuối bên phải của khoảng hiện tại. Đây là vị trí tham lam để duy trì sự tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 2
2 5
```Sau khi sắp xếp theo điểm cuối bên phải:$$[0,2], [2,5]$$| Phân khúc hiện tại | Nail Hiện Có | Được bảo hiểm? | Hành động | Làm Móng Sau Bước | 
| --- | --- | --- | --- | --- | 
| [0,2] | [] | Không | Đặt đinh ở vị trí 2 | [2] | 
| [2,5] | [2] | Có | Bỏ qua | [2] | 

Câu trả lời cuối cùng:```
1
2
```Ví dụ này giải thích tại sao điểm cuối phải được tính là được bảo hiểm. Chiếc đinh ở vị trí 2 nằm trên cả hai khoảng. 

### Ví dụ 2 

đầu vào:```
4
1 4
2 5
3 6
7 8
```Các khoảng được sắp xếp:$$[1,4], [2,5], [3,6], [7,8]$$| Phân khúc hiện tại | Nail Hiện Có | Được bảo hiểm? | Hành động | Làm Móng Sau Bước | 
| --- | --- | --- | --- | --- | 
| [1,4] | [] | Không | Đặt đinh ở vị trí 4 | [4] | 
| [2,5] | [4] | Có | Bỏ qua | [4] | 
| [3,6] | [4] | Có | Bỏ qua | [4] | 
| [7,8] | [4] | Không | Đặt đinh ở vị trí 8 | [4,8] | 

Câu trả lời cuối cùng:```
2
4 8
```Dấu vết này cho thấy lợi ích của việc đặt đinh ở đúng điểm cuối. Một chiếc đinh ở số 4 bao phủ cả ba khoảng chồng chéo, trong khi việc chọn điểm cuối bên trái một cách tham lam có thể thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp thống trị thời gian chạy | 
| Không gian |$O(n)$| Lưu trữ các phân đoạn và danh sách câu trả lời | 

Chỉ với 1000 khoảng thời gian, giải pháp này dễ dàng nằm trong giới hạn. Việc sắp xếp 1000 phần tử là chuyện nhỏ và việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    
    out = io.StringIO()
    sys.stdout = out
    
    n = int(input())
    
    segments = []
    
    for _ in range(n):
        l, r = map(int, input().split())
        
        if l > r:
            l, r = r, l
        
        segments.append((l, r))
    
    segments.sort(key=lambda x: x[1])
    
    nails = []
    
    for l, r in segments:
        if nails and l <= nails[-1] <= r:
            continue
        
        nails.append(r)
    
    print(len(nails))
    print(*nails)
    
    return out.getvalue()

# provided sample
assert run(
"""2
0 2
2 5
"""
) == "1\n2\n", "sample 1"

# minimum-size input
assert run(
"""1
3 3
"""
) == "1\n3\n", "single point segment"

# reversed endpoints
assert run(
"""2
5 1
2 4
"""
) == "1\n4\n", "endpoint normalization"

# disjoint intervals
assert run(
"""3
1 2
4 5
7 8
"""
) == "3\n2 5 8\n", "all intervals separate"

# overlapping chain
assert run(
"""4
1 4
2 5
3 6
7 8
"""
) == "2\n4 8\n", "greedy right endpoint"

# touching endpoints
assert run(
"""3
1 2
2 3
3 4
"""
) == "2\n2 4\n", "closed interval boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khoảng thời gian đơn | Một cái đinh ở điểm đó | Phân đoạn thoái hóa | 
| Điểm cuối đảo ngược | Chuẩn hóa đúng | Độ bền đầu vào | 
| Khoảng cách hoàn toàn rời rạc | Một móng tay mỗi khoảng thời gian | Kích thước câu trả lời trong trường hợp xấu nhất | 
| Chuỗi chồng chéo | Bảo hiểm được chia sẻ | Sự đúng đắn tham lam | 
| Chạm vào điểm cuối | Bao gồm điểm cuối | Xử lý khoảng thời gian đóng | 

## Vỏ cạnh 

Xem xét các điểm cuối đảo ngược:```
2
5 1
2 4
```Sau khi chuẩn hóa, các khoảng trở thành$[1,5]$Và$[2,4]$. Sắp xếp mang lại:$$[2,4], [1,5]$$Thuật toán đặt đinh ở vị trí thứ 4 trong khoảng thời gian đầu tiên. Khoảng thứ hai cũng chứa 4 nên không cần đóng thêm đinh. 

Đầu ra:```
1
4
```Nếu không chuẩn hóa, khoảng$(5,1)$sẽ hành xử không đúng trong quá trình phân loại và kiểm tra ngăn chặn. 

Bây giờ hãy xem xét các khoảng suy biến:```
3
1 1
1 3
5 5
```Thứ tự sắp xếp:$$[1,1], [1,3], [5,5]$$Thuật toán trước tiên đặt đinh ở mức 1. Điều đó bao gồm cả khoảng điểm và khoảng lớn hơn. Sau đó nó đặt một chiếc đinh khác ở vị trí 5. 

Đầu ra:```
2
1 5
```Điều này xác nhận rằng các khoảng có độ dài bằng 0 hoạt động một cách tự nhiên với cùng một logic tham lam. 

Cuối cùng, hãy xem xét các khoảng thời gian chạm:```
3
1 2
2 3
3 4
```Thứ tự sắp xếp không thay đổi. 

Thuật toán đặt đinh ở vị trí thứ 2 trong khoảng thời gian đầu tiên. Khoảng thứ hai đã được bao phủ bởi vì nó bao gồm 2. Khoảng thứ ba không chứa 2, vì vậy một chiếc đinh khác được đặt ở vị trí 4. 

Đầu ra:```
2
2 4
```Điều này chứng tỏ tại sao việc đưa vào điểm cuối lại quan trọng. Việc xử lý các khoảng thời gian như mở sẽ không chính xác khi yêu cầu ba chiếc đinh.
