---
title: "CF 104069F - Hàng đợi thực phẩm"
description: "Chúng tôi được phân theo từng nhóm sinh viên, mỗi sinh viên được giao cho một trong bốn quầy dịch vụ độc lập và mỗi sinh viên có một khoảng thời gian cố định cần thiết để được phục vụ. Mỗi quầy hoạt động độc lập và chỉ có thể phục vụ một học sinh tại một thời điểm."
date: "2026-07-02T03:00:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "F"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 48
verified: true
draft: false
---

[CF 104069F - Hàng thực phẩm](https://codeforces.com/problemset/problem/104069/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phân theo từng nhóm sinh viên, mỗi sinh viên được giao cho một trong bốn quầy dịch vụ độc lập và mỗi sinh viên có một khoảng thời gian cố định cần thiết để được phục vụ. Mỗi quầy hoạt động độc lập và chỉ có thể phục vụ một học sinh tại một thời điểm. Tất cả các quầy đóng đồng thời sau một thời gian cố định$T$. Một học sinh hợp lệ nếu họ có thể được xếp lịch tại quầy được chỉ định sao cho toàn bộ dịch vụ của họ kết thúc trước hoặc chính xác vào thời gian$T$. 

Quyền tự do chính là trong mỗi bộ đếm, chúng tôi không bị buộc phải tôn trọng thứ tự đầu vào. Chúng ta có thể sắp xếp lại thứ tự học sinh tùy ý trên mỗi quầy để tối đa hóa số lượng học sinh hoàn thành đúng thời gian. 

Vì vậy, đối với mỗi bộ đếm trong số bốn bộ đếm, chúng ta đang giải quyết vấn đề lập kế hoạch: cho một tập hợp thời gian xử lý trên một máy có thời hạn$T$, tối đa hóa số lượng nhiệm vụ có thể được hoàn thành tuần tự. 

Kích thước đầu vào có thể lớn như$4 \cdot 10^5$, do đó bất kỳ bậc hai hoặc thậm chí$O(N \log N)$giải pháp phải được cấu trúc cẩn thận. Cho phép sắp xếp nhưng mọi thao tác liên quan đến quét lồng nhau hoặc tính toán lại lặp đi lặp lại cho mỗi tập hợp con ứng cử viên sẽ không thành công. 

Một trường hợp phức tạp xuất hiện khi các nhiệm vụ lớn lấn át các nhiệm vụ nhỏ. Ví dụ, nếu$T = 10$và một bộ đếm có thời gian$[9, 9, 1]$, một kẻ tham lam ngây thơ không sắp xếp lại chính xác có thể chọn$9 + 1$hoặc thậm chí không coi việc đặt hàng là quan trọng. Câu trả lời đúng là$2$bởi vì chúng ta có thể lên lịch$1 + 9$. Bất kỳ cách tiếp cận nào tôn trọng thứ tự đầu vào sẽ kết luận sai rằng chỉ có một học sinh phù hợp. 

Một trường hợp khác là khi tất cả các tác vụ chỉ phù hợp sau khi sắp xếp. Ví dụ,$T = 10$, lần$[8, 7, 3]$. Lựa chọn tối ưu là$3 + 7 = 10$, không$8$một mình, điều này một lần nữa cho thấy việc đặt hàng là cần thiết. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo trực tiếp sẽ thử từng tập hợp con học sinh cho mỗi bộ đếm và đối với mỗi tập hợp con, hãy kiểm tra xem chúng có thể được sắp xếp phù hợp theo thời gian hay không$T$. Ngay cả khi bỏ qua sự phức tạp của việc đặt hàng, điều này đã ngụ ý$2^{N}$tập hợp con, điều này là không thể đối với$N = 4 \cdot 10^5$. Thậm chí hạn chế ở một quầy duy nhất với$n$các phần tử, việc kiểm tra tất cả các tập hợp con là theo cấp số nhân. 

Chúng tôi tinh chỉnh quan điểm. Đối với một bộ đếm cố định, giả sử chúng ta quyết định phân phối chính xác$k$sinh viên. Cách tốt nhất để giảm thiểu tổng thời gian hoàn thành là chọn$k$khoảng thời gian nhỏ nhất. Đây là một đối số trao đổi cổ điển: nếu một tập hợp được chọn chứa phần tử lớn hơn trong khi tồn tại một phần tử nhỏ hơn không được sử dụng, thì việc hoán đổi chúng không bao giờ làm tăng tổng thời gian. 

Do đó mỗi bộ đếm trở nên độc lập. Đối với mỗi cái, chúng tôi sắp xếp thời gian phục vụ của nó và tham lam lấy tiền tố nhỏ nhất cho đến khi tổng tích lũy vượt quá$T$. Câu trả lời là tổng của bốn kết quả độc lập này. 

Sự đơn giản hóa chính là không có sự tương tác giữa các bộ đếm. Mỗi học sinh thuộc về chính xác một máy, vì vậy chúng tôi giải quyết bốn vấn đề độc lập “tối đa hóa tiền tố theo ràng buộc tổng”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi bộ đếm trong số bốn bộ đếm là một nhóm thời gian xử lý riêng biệt. 

1. Chia tất cả học sinh thành bốn danh sách theo số đếm được chỉ định. Điều này tách bài toán lập kế hoạch thành các bài toán con độc lập vì không học sinh nào có thể di chuyển giữa các bộ đếm. 
2. Đối với mỗi quầy, sắp xếp danh sách theo thứ tự thời gian phục vụ tăng dần. Việc sắp xếp là cần thiết vì thực hiện các nhiệm vụ nhỏ hơn trước tiên luôn để lại nhiều chỗ hơn cho các học sinh bổ sung trong quỹ thời gian cố định$T$. 
3. Duyệt qua danh sách đã sắp xếp trong khi vẫn duy trì tổng số lần đã chọn. Bắt đầu từ số 0 và liên tục thêm vào thời điểm nhỏ nhất tiếp theo. 
4. Bất cứ khi nào thêm lần sau sẽ làm cho số tiền vượt quá$T$, dừng lại ngay lập tức. Tất cả các nhiệm vụ còn lại đều lớn hơn hoặc bằng nhau nên không thể cải thiện số lượng. 
5. Đếm xem có bao nhiêu nhiệm vụ đã được thêm thành công trước khi vượt quá$T$và thêm giá trị này vào câu trả lời chung. 
6. Tổng kết quả trên cả bốn quầy và xuất ra tổng số cuối cùng. 

Tại sao nó hoạt động: trong mỗi bộ đếm, bất kỳ lịch trình khả thi nào cũng có thể được sắp xếp lại thành thứ tự không giảm mà không làm thay đổi tính khả thi, bởi vì việc hoán đổi một nhiệm vụ lớn hơn trước đó chỉ làm tăng hoặc bảo toàn tổng tiền tố. Do đó, giải pháp tối ưu luôn tương ứng với việc lấy tiền tố của mảng đã được sắp xếp và dừng ở điểm đầu tiên mà tổng tiền tố vượt quá$T$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, T = map(int, input().split())
    
    groups = {"C": [], "F": [], "P": [], "Q": []}
    
    for _ in range(N):
        b, v = input().split()
        v = int(v)
        groups[b].append(v)
    
    ans = 0
    
    for k in groups:
        arr = groups[k]
        arr.sort()
        
        s = 0
        cnt = 0
        
        for x in arr:
            if s + x > T:
                break
            s += x
            cnt += 1
        
        ans += cnt
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là phân loại học sinh theo quầy được chỉ định. Điều này rất quan trọng vì mỗi bộ đếm hoạt động giống như một máy độc lập có hàng đợi riêng. Việc sắp xếp từng nhóm đảm bảo chúng tôi luôn xem xét khoảng thời gian nhỏ nhất trước tiên, đây là thứ tự duy nhất có thể tối đa hóa số lượng nhiệm vụ trong một ràng buộc về tổng. 

Tổng số tiền chạy`s`theo dõi thời gian hoàn thành hiện tại nếu chúng tôi phân phát tiền tố đã chọn. khoảnh khắc`s + x`vượt quá$T$, chúng tôi dừng lại vì việc thêm bất kỳ phần tử lớn hơn nào sẽ chỉ làm xấu đi tính khả thi. 

Câu trả lời chung tích lũy kết quả từ cả bốn quầy vì chúng hoạt động độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 10
C 5
F 3
P 2
Q 4
C 1
C 6
```Sau khi phân nhóm: 

C = [5, 1, 6], F = [3], P = [2], Q = [4] 

Đã sắp xếp: 

C = [1, 5, 6], F = [3], P = [2], Q = [4] 

Dấu vết truy cập C: 

| Bước | Giá trị | Tổng Chạy | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | lấy | 
| 2 | 5 | 6 | lấy | 
| 3 | 6 | 12 | dừng lại | 

F mất 1, P mất 1, Q mất 1. 

Tổng cộng = 2 + 1 + 1 + 1 = 5. 

Điều này cho thấy rằng mỗi bộ đếm hoạt động độc lập và đóng góp tiền tố tối đa của riêng nó, mặc dù các tác vụ tổng thể được xen kẽ theo thứ tự đầu vào. 

### Ví dụ 2 

đầu vào:```
4 10
C 6
C 3
C 4
C 5
```Nhóm: 

C = [6, 3, 4, 5] → được sắp xếp [3, 4, 5, 6] 

Dấu vết: 

| Bước | Giá trị | Tổng Chạy | Hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | lấy | 
| 2 | 4 | 7 | lấy | 
| 3 | 5 | 12 | dừng lại | 

Câu trả lời là 2. 

Điều này chứng tỏ việc lựa chọn các phần tử nhỏ nhất trước tiên là cần thiết; chỉ lấy 6 sẽ dẫn đến kết quả tồi tệ hơn so với lấy 3 và 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi học sinh được xếp vào một nhóm và mỗi nhóm được sắp xếp một lần | 
| Không gian |$O(N)$| Chúng tôi lưu trữ tất cả học sinh được nhóm theo quầy | 

Chi phí phân loại chiếm ưu thế, nhưng với$N \le 4 \cdot 10^5$, điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    def solve():
        N, T = map(int, input().split())
        groups = {"C": [], "F": [], "P": [], "Q": []}
        for _ in range(N):
            b, v = input().split()
            v = int(v)
            groups[b].append(v)
        ans = 0
        for k in groups:
            arr = sorted(groups[k])
            s = 0
            cnt = 0
            for x in arr:
                if s + x > T:
                    break
                s += x
                cnt += 1
            ans += cnt
        print(ans)

    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case
assert run("""6 10
C 5
F 3
P 2
Q 4
C 1
C 6
""") == "5"

# minimum case
assert run("""1 1
C 1
""") == "1"

# impossible case
assert run("""3 2
C 3
C 4
C 5
""") == "0"

# all fit
assert run("""3 100
C 1
C 2
C 3
""") == "3"

# mixed counters
assert run("""4 5
C 3
F 3
P 2
Q 2
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống mẫu | 5 | chính xác trên nhiều quầy | 
| 1 1/1 | 1 | đầu vào tối thiểu | 
| tất cả > T | 0 | không có nhiệm vụ khả thi | 
| thời gian nhỏ | 3 | chấp nhận hoàn toàn trong ngân sách | 
| quầy hỗn hợp | 4 | sự độc lập của nhóm | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi một bộ đếm có nhiều giá trị lớn và một vài giá trị nhỏ. Ví dụ:```
C = [100, 1, 1, 1], T = 3
```Sắp xếp cho [1, 1, 1, 100]. Thuật toán mất 3 giây và dừng trước 100, tạo ra 3. Một cách tiếp cận đơn giản xử lý thứ tự đầu vào có thể mất 100 đầu tiên và chỉ kết luận sai một học sinh. 

Trường hợp cạnh thứ hai là khi tất cả các giá trị khớp chính xác. Ví dụ:```
C = [2, 3, 5], T = 10
```Thứ tự sắp xếp cung cấp sự bao gồm đầy đủ. Tổng số tiền đạt chính xác là 10 và tất cả đều được thực hiện. Tình trạng ngắt chỉ kích hoạt khi vượt quá$T$, do đó, sự bình đẳng được xử lý chính xác mà không có lỗi nào xảy ra.
