---
problem: 980C
contest_id: 980
problem_index: C
name: "Posterized"
contest_name: "Codeforces Round 480 (Div. 2)"
rating: 1700
tags: ["games", "greedy"]
answer: passed_samples
verified: true
solve_time_s: 67
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a56a-75ec-83ec-8c82-16909b3df43c
---

#CF 980C - Posterized 

**Đánh giá:** 1700 
**Tags:** trò chơi, tham lam 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a56a-75ec-83ec-8c82-16909b3df43c 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một chuỗi cường độ pixel, mỗi chuỗi là một số nguyên từ 0 đến 255. Nhiệm vụ là “đơn giản hóa” chuỗi này bằng cách trước tiên phân vùng toàn bộ phạm vi giá trị$[0, 255]$thành các đoạn liền kề. Mỗi đoạn có độ dài tối đa$k$và mọi số nguyên trong phạm vi phải thuộc chính xác một phân đoạn. Đối với mỗi phân đoạn, chúng tôi chọn một giá trị đại diện được gọi là khóa của nó. Sau đó, mọi giá trị pixel được thay thế bằng khóa của phân đoạn mà nó thuộc về. 

Đầu ra cuối cùng là mảng pixel được chuyển đổi. Trong số tất cả các cách hợp lệ để phân vùng phạm vi và chọn khóa, chúng tôi muốn mảng kết quả nhỏ nhất về mặt từ điển. 

Tương tác quan trọng là phép biến đổi chỉ phụ thuộc vào việc phân đoạn các giá trị chứ không phụ thuộc vào các vị trí trong mảng. Khi chúng tôi quyết định giá trị nào ánh xạ tới khóa nào, tất cả các lần xuất hiện của giá trị đó sẽ được thay thế một cách nhất quán. 

Các ràng buộc nhỏ trên miền giá trị (chỉ có 256 màu có thể), nhưng lớn trên chiều dài mảng$n \le 10^5$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào cũng nên tính toán trước ánh xạ cho tất cả 256 giá trị và sau đó áp dụng nó theo thời gian tuyến tính trên mảng. 

Một cách tiếp cận ngây thơ sẽ thử tất cả các phân đoạn của$[0,255]$. Vì mỗi đoạn có độ dài tối đa$k$, điều này trở thành một vấn đề phân vùng tổ hợp với các lựa chọn theo cấp số nhân. Mặc dù 256 là nhỏ, việc phân nhánh trên tất cả các ranh giới phân khúc có thể vẫn dẫn đến sự bùng nổ về khả năng. 

Ý tưởng ngây thơ thứ hai là tham lam chọn một phân vùng giúp giảm thiểu mảng được chuyển đổi mà không xem xét các tương tác toàn cục. Đây là nơi xảy ra những lỗi nhỏ: các quyết định ban đầu về ranh giới phân đoạn sẽ ảnh hưởng đến các khóa trong tương lai và một lựa chọn tối ưu cục bộ có thể làm tăng giá trị từ điển sau này. 

Một kịch bản lỗi điển hình xuất phát từ việc chọn khóa phân đoạn không phải là khóa tối thiểu trong khối của nó. Ví dụ, nếu chúng ta nhóm$[2,3,4]$có chìa khóa$3$, nhưng chúng ta có thể chọn khóa$2$, thì mỗi lần xuất hiện của 2 sẽ trở nên tệ hơn theo thứ tự từ điển. Ngay cả khi các phân đoạn sau cải thiện các vị trí khác, thứ tự từ điển vẫn được quyết định ở chỉ mục khác nhau đầu tiên. 

Vì vậy khó khăn chính là: ranh giới phân đoạn và các khóa được chọn phải được phối hợp để tối ưu hóa thứ tự từ điển trên toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi phân vùng hợp lệ của$[0,255]$thành các đoạn có độ dài tối đa$k$. Đối với mỗi phân vùng, chúng tôi sẽ thử mọi lựa chọn có thể có của khóa bên trong mỗi phân đoạn và tính toán ánh xạ kết quả cho tất cả 256 giá trị, sau đó áp dụng nó vào mảng và so sánh kết quả theo từ điển. Ngay cả khi chúng ta chỉ xem xét các phân đoạn, số cách để cắt một mảng có độ dài 256 với kích thước khối tối đa$k$tăng theo cấp số nhân vào năm 256, khiến điều này không thể thực hiện được. 

Quan sát quan trọng là miền giá trị là cố định và nhỏ, vì vậy chúng ta không nên nghĩ về các vị trí trong mảng mà về mặt ánh xạ từng giá trị$v \in [0,255]$cho người đại diện cuối cùng của nó. Khi ánh xạ này được sửa, việc xây dựng đầu ra rất đơn giản. 

Bây giờ hãy xem xét thứ tự từ điển của mảng cuối cùng hoạt động như thế nào. Vị trí sớm nhất trong mảng khác nhau hoàn toàn phụ thuộc vào giá trị pixel nhỏ nhất nhận được kết quả được ánh xạ khác. Điều này gợi ý rằng các giá trị nhỏ hơn nên được xử lý càng sớm và càng thuận lợi càng tốt. 

Trong bất kỳ phân khúc nào, việc chọn khóa lớn hơn chỉ có thể làm cho giá trị kém hơn hoặc bằng so với việc chọn khóa nhỏ hơn trong cùng phân khúc. Vì vậy, đối với một đoạn cố định, lựa chọn tốt nhất luôn là giá trị nhỏ nhất của nó. 

Điều này làm giảm vấn đề chỉ chọn ranh giới phân khúc. Mỗi đoạn$[l, r]$(với$r - l + 1 \le k$) ánh xạ mọi giá trị trong đó tới$\min(l, r)$là không đúng; thay vào đó, nó ánh xạ tới$\min$của các giá trị có trong phân đoạn đó về mặt khái niệm, nhưng vì tất cả các số nguyên đều tồn tại trong phạm vi nên đại diện tốt nhất chỉ đơn giản là$l$. Vì vậy, mỗi phân đoạn sẽ trở thành một khối trong đó tất cả các giá trị ánh xạ tới điểm cuối bên trái của nó. 

Bây giờ chúng ta phải chọn nơi để cắt phạm vi. Chiến lược tham lam là luôn lấy phân đoạn lớn nhất có thể bắt đầu từ 0 để cải thiện thứ tự từ điển sớm nhất. Nhưng sự đơn giản hóa quan trọng thậm chí còn mạnh mẽ hơn: chúng ta có thể quyết định ánh xạ cho từng giá trị theo thứ tự tăng dần, luôn đảm bảo rằng chúng ta mở rộng một phân khúc đến hết kích thước có thể$k$, bởi vì việc trì hoãn việc cắt chỉ buộc các giá trị lớn hơn phải chia sẻ một đại diện nhỏ hơn sớm hơn, điều này luôn tối ưu về mặt từ điển. 

Điều này dẫn đến một tham lam đơn giản: chúng tôi duyệt qua các giá trị từ 0 đến 255, gán từng giá trị cho phân khúc hiện tại và bất cứ khi nào phân khúc đạt kích thước$k$, chúng ta bắt đầu một cái mới. Khóa của mỗi phân đoạn là giá trị đầu tiên của nó. 

Điều này tạo ra một ánh xạ cố định của tất cả 256 giá trị và sau đó chúng tôi áp dụng nó cho mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | Hàm mũ | O(1) | Quá chậm | 
| Phân khúc có kích thước cố định tham lam | O(256 + n) | O(256) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng ánh xạ từ mỗi giá trị$0 \ldots 255$đến giá trị nén cuối cùng của nó. 

1. Khởi tạo một mảng ánh xạ trống`mp`có kích thước 256. Đặt một biến`start = 0`, đánh dấu sự bắt đầu của phân đoạn hiện tại. 
2. Lặp lại các giá trị`v`từ 0 đến 255. 
3. Chỉ định`mp[v] = start`, nghĩa là tất cả các giá trị trong phân đoạn hiện tại ánh xạ tới điểm bắt đầu của phân đoạn. 
4. Nếu chúng tôi đã chỉ định$k$giá trị kể từ khi bắt đầu phân đoạn cuối cùng, chúng tôi di chuyển`start = v + 1`, bắt đầu một đoạn mới. 

Điều này đảm bảo mỗi phân khúc có kích thước tối đa$k$, và chúng tôi luôn chọn người đại diện sớm nhất có thể. 
5. Sau khi xử lý tất cả các giá trị, chúng tôi có bản đồ hoàn chỉnh cho mọi cường độ điểm ảnh có thể có. 
6. Chuyển đổi mảng đầu vào bằng cách thay thế từng giá trị`p[i]`với`mp[p[i]]`. 

Lý do điều này có hiệu quả là khi một giá trị được gán cho một phân đoạn bắt đầu từ`start`, việc gán cho nó bất kỳ đại diện lớn hơn nào sẽ chỉ làm tăng giá trị trong mảng cuối cùng ở tất cả các vị trí mà nó xuất hiện. Vì thứ tự từ điển được quyết định từ trái sang phải, nên việc làm cho bất kỳ giá trị ban đầu nào lớn hơn sẽ không thể được bù lại sau này. Phân đoạn tham lam đảm bảo chúng tôi không bao giờ trì hoãn ranh giới theo cách có thể làm tăng các giá trị được ánh xạ trước đó. 

Điều bất biến là đối với mọi tiền tố giá trị được xử lý, ánh xạ đã tối ưu cho tất cả các mảng chỉ chứa các giá trị đó, bởi vì mỗi phân đoạn luôn sử dụng đại diện nhỏ nhất có thể và không bao giờ vượt quá kích thước$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    
    mp = [0] * 256
    
    start = 0
    cnt = 0
    
    for v in range(256):
        mp[v] = start
        cnt += 1
        if cnt == k:
            start = v + 1
            cnt = 0
    
    res = [str(mp[x]) for x in arr]
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng ánh xạ trên miền cố định gồm 256 giá trị pixel có thể. Biến`start`theo dõi ranh giới bên trái của phân khúc hiện tại. Mỗi lần chúng tôi phân công`k`giá trị cho một phân đoạn, chúng tôi đặt lại`start`để bắt đầu một khối mới. 

Bước chuyển đổi là trực tiếp: mỗi pixel được thay thế bằng giá trị ánh xạ được tính toán trước của nó trong thời gian không đổi. 

Một điểm tinh tế là chúng ta không bao giờ cần lưu trữ phạm vi phân đoạn một cách rõ ràng. Cặp đôi`(start, cnt)`ngầm xác định phân khúc hiện tại. Ngoài ra, chúng ta không cần phải lo lắng về việc kết thúc chính xác ở 255 với một phân đoạn đầy đủ, bởi vì các phân đoạn một phần được cho phép và xử lý tự động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
2 14 3 4
```Chúng tôi tính toán ánh xạ trên các giá trị: 

| v | bắt đầu | cnt | mp[v] | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 0 | 
| 1 | 0 | 2 | 0 | 
| 2 | 0 | 3 → đặt lại | 0 | 
| 3 | 3 | 1 | 3 | 
| 4 | 3 | 2 | 3 | 
| 5 | 3 | 3 → đặt lại | 3 | 
| ... | ... | ... | ... | 
| 12 | 12 | 1 | 12 | 
| 13 | 12 | 2 | 12 | 
| 14 | 12 | 3 | 12 | 

Áp dụng bản đồ:```
2 → 0
14 → 12
3 → 3
4 → 3
```Đầu ra:```
0 12 3 3
```Điều này xác nhận rằng mỗi giá trị được ánh xạ tới điểm bắt đầu phân đoạn của nó. 

### Ví dụ 2 

đầu vào:```
5 2
0 1 2 3 4
```Lập bản đồ: 

| v | mp[v] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 0 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 4 | 

Đầu ra:```
0 0 2 2 4
```Điều này cho thấy các phân đoạn nhỏ hơn buộc phải phân nhóm sớm hơn như thế nào và tạo ra một trình tự tối thiểu về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 256) | Một lần vượt qua phạm vi giá trị và một lần vượt qua mảng đầu vào | 
| Không gian | O(256) | Đã sửa lỗi mảng ánh xạ | 

Giải pháp này phù hợp thoải mái với các ràng buộc vì tính toán nặng không phụ thuộc vào$n$và việc xử lý mảng là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))
    
    mp = [0] * 256
    start = 0
    cnt = 0
    for v in range(256):
        mp[v] = start
        cnt += 1
        if cnt == k:
            start = v + 1
            cnt = 0
    
    return " ".join(str(mp[x]) for x in arr)

# provided sample
assert run("4 3\n2 14 3 4\n") == "0 12 3 3"

# all equal
assert run("5 2\n7 7 7 7 7\n") == "6 6 6 6 6"

# k = 1 (identity mapping)
assert run("3 1\n1 2 3\n") == "1 2 3"

# maximum k
assert run("4 256\n0 100 200 255\n") == "0 0 0 0"

# boundary around segment break
assert run("6 3\n0 1 2 3 4 5\n") == "0 0 0 3 3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 | danh tính | không cho phép nén | 
| k=256 | phân đoạn đơn | sụp đổ hoàn toàn | 
| nghỉ liên tiếp | khối được nhóm lại | độ đúng ranh giới | 
| tất cả đều bình đẳng | ổn định | lập bản đồ thống nhất | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$k = 1$. Thuật toán buộc mọi giá trị phải bắt đầu một phân đoạn mới ngay lập tức, do đó`mp[v] = v`. Điều này tạo ra ánh xạ nhận dạng, điều này đúng vì không có hai giá trị riêng biệt nào có thể chia sẻ một nhóm. 

Một trường hợp khác là$k = 256$, trong đó toàn bộ phạm vi trở thành một phân đoạn duy nhất. Ở đây tất cả các giá trị ánh xạ tới 0, vì phân đoạn bắt đầu từ 0 và không bao giờ đặt lại. Thuật toán tạo ra một mảng được làm phẳng hoàn toàn một cách chính xác. 

Một trường hợp tinh tế hơn là khi các giá trị trong đầu vào thưa thớt, ví dụ: chỉ có các giá trị lớn như 200 đến 210. Ánh xạ vẫn được xác định trên toàn cầu trong toàn bộ phạm vi từ 0 đến 255, do đó các giá trị này có thể rơi vào các phân đoạn khác nhau so với dự kiến. Thuật toán vẫn hoạt động vì phân đoạn được cố định độc lập với phân phối đầu vào, điều này được yêu cầu bởi định nghĩa vấn đề.
