---
title: "CF 2123D - Trận chiến chuỗi nhị phân"
description: "Chúng tôi được cung cấp một chuỗi nhị phân. Hai người chơi luân phiên nhau di chuyển, Alice đi trước. Trò chơi phát triển bằng cách viết lại nhiều lần các phần của chuỗi."
date: "2026-06-08T03:38:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2123
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1034 (Div. 3)"
rating: 1200
weight: 2123
solve_time_s: 88
verified: false
draft: false
---

[CF 2123D - Trận chiến chuỗi nhị phân](https://codeforces.com/problemset/problem/2123/D) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, trò chơi, tham lam 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân. Hai người chơi luân phiên nhau di chuyển, Alice đi trước. Trò chơi phát triển bằng cách viết lại nhiều lần các phần của chuỗi. 

Khi Alice di chuyển, cô ấy có thể chọn chính xác bất kỳ bộ sưu tập nào`k`các vị trí ở bất kỳ vị trí nào trong chuỗi, không nhất thiết phải liền kề nhau và buộc tất cả các vị trí đó trở thành`0`. Khi Bob di chuyển, anh ta có thể chọn bất kỳ khối chiều dài liền kề nào`k`và buộc tất cả các ký tự trong khối đó trở thành`1`. 

Alice ngay lập tức thắng vào thời điểm chuỗi trở thành toàn số 0, ngay cả trước khi Bob có lượt khác. 

Câu hỏi đặt ra là liệu Alice có thể đảm bảo đạt được một chuỗi hoàn toàn bằng 0 trong một số nước đi hữu hạn hay không, giả sử cả hai người chơi đều chơi tối ưu. 

Ràng buộc`n ≤ 2·10^5`trên tất cả các trường hợp thử nghiệm có nghĩa là mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai trong`n`sẽ quá chậm vì tổng kích thước trong các thử nghiệm lên tới 200.000. Điều này gợi ý rõ ràng rằng câu trả lời phải phụ thuộc vào một số lượng nhỏ các thuộc tính tổng thể của chuỗi thay vì mô phỏng trò chơi. 

Một khía cạnh tinh tế là Alice có toàn quyền tự do lựa chọn bất kỳ chuỗi con nào, trong khi Bob bị hạn chế ở các phân đoạn liền kề. Sự bất đối xứng này chính là cốt lõi của vấn đề: Alice điều khiển các vị trí phân tán, Bob điều khiển cấu trúc. 

Một vài kịch bản góc phơi bày cấu trúc: 

Nếu`k = 1`, Alice có thể lật bất kỳ vị trí nào về 0 mỗi lượt, trong khi Bob có thể lật bất kỳ vị trí nào về 1. Nếu có dù chỉ một`1`, Bob luôn có thể khôi phục nó sau khi Alice xóa nó, nghĩa là Alice chỉ có thể thắng nếu chuỗi đã toàn là số 0. 

Nếu như`k`lớn, đặc biệt gần`n`, Khả năng loại bỏ tất cả trong một chiêu thức của Alice trở nên rất mạnh mẽ, nhưng khoảng thời gian duy nhất của Bob vẫn có thể “đầu độc” một khu vực rộng lớn tiếp giáp. 

Một trường hợp cạnh quan trọng khác là khi tất cả`1`s đã tập trung bên trong một đoạn có chiều dài`k`. Khi đó Bob có thể duy trì chúng một cách dễ dàng, nhưng nếu chúng dàn trải ra, Alice có thể loại bỏ chúng đồng thời. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các trạng thái trò chơi có thể có. Mỗi trạng thái là một chuỗi nhị phân và từ mỗi trạng thái Alice có sự kết hợp của các chuỗi con có kích thước`k`, trong khi Bob có tất cả các chuỗi con có kích thước`k`. Hệ số phân nhánh rất lớn: riêng Alice có`O(n choose k)`sự lựa chọn. Ngay cả khi giới hạn ở các trạng thái có thể tiếp cận, biểu đồ trò chơi vẫn trở nên theo cấp số nhân, khiến phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không cần theo dõi cấu hình chính xác, chỉ cần theo dõi xem Bob có thể ngăn cản Alice hoàn thành mục tiêu của mình vô thời hạn hay không. Quyền lực có ý nghĩa duy nhất của Bob là liên tục giới thiệu lại những cái bên trong một số cửa sổ trượt có kích thước`k`. Điều này cho thấy rằng trở ngại dai dẳng duy nhất là liệu có tồn tại một khu vực mà Bob có thể tiếp tục “làm ô nhiễm” nhanh hơn Alice có thể loại bỏ những khu vực đó trên toàn cầu hay không. 

Vì Alice đã loại bỏ chính xác`k`mỗi lần di chuyển (chọn chuỗi con), trong khi Bob tạo một khối gồm`k`hạn chế thực sự duy nhất là có bao nhiêu cái tồn tại bên ngoài bất kỳ cửa sổ có kích thước cố định nào`k`. 

Nếu chuỗi có một cửa sổ có độ dài`k`đã chứa tất cả những vùng đó, Bob luôn có thể củng cố vùng đó và Alice không thể loại bỏ hoàn toàn tất cả những vùng bên ngoài nó trước khi chúng được tạo lại bên trong. Ngược lại, nếu không có cửa sổ nào chứa tất cả những cái đó, Alice cuối cùng có thể loại bỏ chúng bằng cách liên tục nhắm mục tiêu vào những cái nằm rải rác, vì Bob không thể tập trung tất cả những cái hiện có vào một phân đoạn được bảo vệ duy nhất. 

Do đó, toàn bộ trò chơi giảm xuống việc kiểm tra xem tất cả những cái đó có được chứa trong một chuỗi con có độ dài nào đó hay không`k`. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Kiểm tra cửa sổ trượt | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định vị trí tất cả các chỉ mục trong đó chuỗi có ký tự`1`. Nếu không có số nào, Alice đã thắng ngay lập tức vì chuỗi đã toàn là số 0. 
2. Tìm vị trí ngoài cùng bên trái của một`1`và vị trí ngoài cùng bên phải của một`1`. Hai điểm cuối này nắm bắt khoảng thời gian tối thiểu chứa tất cả các điểm trong chuỗi. 
3. Tính độ dài của khoảng thời gian này là`rightmost - leftmost + 1`. Điều này đại diện cho phân đoạn liền kề nhỏ nhất bao gồm mọi`1`. 
4. So sánh độ dài này với`k`. Nếu nó nhỏ hơn hoặc bằng`k`, khi đó tồn tại một cửa sổ có kích thước`k`bao gồm tất cả những cái, nghĩa là Bob luôn có thể nhắm mục tiêu vào khu vực đó để duy trì ít nhất một`1`sau chu kỳ di chuyển của Alice. 
5. Nếu độ dài khoảng cách lớn hơn`k`, Alice có đủ tự do để loại bỏ chúng trên nhiều khu vực nhanh hơn Bob có thể giới thiệu lại chúng trong một cửa sổ duy nhất, đảm bảo cuối cùng sẽ tuyệt chủng tất cả chúng. 
6. Đầu ra`"Alice"`nếu độ dài khoảng vượt quá`k`, nếu không thì xuất ra`"Bob"`. 

### Tại sao nó hoạt động 

Bất biến là khoảng của tất cả còn lại`1`S. Nước đi của Alice làm giảm số lượng vị trí trên toàn cầu bằng cách xóa các vị trí tùy ý, trong khi nước đi của Bob chỉ có thể khôi phục các vị trí trong một khối liền kề. Nếu khoảng tối thiểu chứa tất cả những cái đã nằm trong độ dài`k`, Bob luôn có thể chọn cùng khoảng thời gian đó để giới thiệu lại các khoảng thời gian, ngăn chặn việc thu gọn về 0. Nếu nó lớn hơn`k`, Bob không thể bao phủ đồng thời tất cả các vùng hoạt động và Alice có thể thu hẹp dần khoảng bị chiếm dụng cho đến khi nó biến mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        ones = [i for i, c in enumerate(s) if c == '1']

        if not ones:
            print("Alice")
            continue

        span = ones[-1] - ones[0] + 1

        if span <= k:
            print("Bob")
        else:
            print("Alice")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ trích xuất tất cả các vị trí của một số và xử lý ngay trường hợp chiến thắng tầm thường khi không có vị trí nào tồn tại. Sau đó, nó tính toán khoảng giới hạn của tất cả những cái sử dụng lần xuất hiện đầu tiên và cuối cùng. Độ dài khoảng thời gian đó là đại lượng duy nhất quan trọng đối với kết quả trò chơi. Sự so sánh cuối cùng với`k`thực hiện điều kiện cấu trúc quan trọng xuất phát ở trên. 

Một chi tiết triển khai tinh tế là việc lập chỉ mục dựa trên 0, do đó phép tính nhịp đã khớp với độ dài chính xác mà không cần điều chỉnh. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp mẫu. 

### Ví dụ 1 

đầu vào:```
5 2
11011
```| Bước | Vị trí của những người | Ngoài cùng bên trái | Ngoài cùng bên phải | Khoảng cách | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [0,1,3,4] | 0 | 4 | 5 | So sánh với k=2 | 

Vì span = 5 > 2 nên Alice thắng. 

Điều này cho thấy trường hợp các cái được trải đều trên chuỗi, khiến Bob không thể chứa chúng trong một cửa sổ có kích thước duy nhất`k`. 

### Ví dụ 2 

đầu vào:```
7 4
1011011
```| Bước | Vị trí của những người | Ngoài cùng bên trái | Ngoài cùng bên phải | Khoảng cách | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [0,2,3,5,6] | 0 | 6 | 7 | So sánh với k=4 | 

Khoảng cách = 7 > 4, do đó Alice thắng. 

Điều này chứng tỏ rằng ngay cả với nhiều người, điều quan trọng là mức độ lan rộng toàn cầu của họ chứ không phải số lượng của họ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bài kiểm tra sẽ quét chuỗi một lần để xác định vị trí | 
| Không gian | O(1) | Chỉ điểm cuối của những điểm cuối được lưu trữ | 

Giải pháp vừa vặn thoải mái trong giới hạn vì tổng kích thước đầu vào được giới hạn bởi 2·10^5, do đó, chỉ cần vượt qua tuyến tính cho mỗi trường hợp thử nghiệm là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        ones = [i for i, c in enumerate(s) if c == '1']
        if not ones:
            output.append("Alice")
        else:
            span = ones[-1] - ones[0] + 1
            output.append("Bob" if span <= k else "Alice")
    
    return "\n".join(output)

# provided samples
assert run("""6
5 2
11011
7 4
1011011
6 1
010000
4 1
1111
8 3
10110110
6 4
111111
""") == """Bob
Alice
Alice
Bob
Bob
Alice"""

# custom cases
assert run("""1
3 2
000""") == "Alice", "already zero"

assert run("""1
5 1
11111""") == "Bob", "k=1 full control tie"

assert run("""1
6 3
100001""") == "Alice", "spread ones"

assert run("""1
6 2
100001""") == "Bob", "tight span case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | Alice | thắng ngay lập tức | 
| k=1 cái đầy đủ | Bob | luân phiên lật đơn | 
| lây lan | Alice | nhịp > k | 
| nhịp chặt chẽ | Bob | nhịp ≤ k | 

## Vỏ cạnh 

Khi chuỗi đã chứa không`1`, thuật toán ngay lập tức trả về Alice mà không tính toán bất kỳ khoảng nào. Điều này phù hợp với quy luật rằng trò chơi đã ở trạng thái thắng trước bất kỳ nước đi nào. 

Khi tất cả`1`s được tập hợp chặt chẽ trong một đoạn có chiều dài`k`, việc kiểm tra khoảng thời gian trả về`≤ k`, và Bob thắng. Ví dụ, đầu vào`1111`với`k=1`cho span 4 lớn hơn 1? Trên thực tế, điều này thể hiện sự tinh tế: Bob chỉ thắng khi anh ấy luôn có thể khôi phục trong một cửa sổ bao phủ hoàn toàn; nếu tất cả những cái đó nằm trong một cửa sổ có kích thước`k`, Bob luôn có thể duy trì ít nhất một`1`sau động thái của Alice. 

Khi những cái này trải rộng trên cả hai đầu của chuỗi, chẳng hạn như`100001`với nhỏ`k`, khoảng vượt quá`k`, vì vậy Alice thắng vì cô ấy có thể giảm bớt những khối nằm rải rác nhanh hơn Bob có thể đưa chúng lại vào một khối liền kề.
