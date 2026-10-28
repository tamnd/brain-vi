---
title: "CF 106161I - Tam giác bên trong"
description: "Mỗi trường hợp thử nghiệm đưa ra một tập hợp các bài báo và mỗi bài báo được đánh giá bởi một nhóm nhỏ người đánh giá. Mọi người đánh giá đều cho điểm số nguyên từ −3 đến 3 và tổng điểm của bài viết chỉ là tổng của các giá trị này."
date: "2026-06-19T19:12:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106161
codeforces_index: "I"
codeforces_contest_name: "The 2025 ICPC Asia Chengdu Regional Contest (The 4rd Universal Cup. Stage 4: Grand Prix of Chengdu)"
rating: 0
weight: 106161
solve_time_s: 70
verified: true
draft: false
---

[CF 106161I - Tam giác bên trong](https://codeforces.com/problemset/problem/106161/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một tập hợp các bài báo và mỗi bài báo được đánh giá bởi một nhóm nhỏ người đánh giá. Mọi người đánh giá đều cho điểm số nguyên từ −3 đến 3 và tổng điểm của bài viết chỉ là tổng của các giá trị này. Một bài viết được coi là chấp nhận nếu tổng số điểm của nó đạt ít nhất một giá trị ngưỡng$k$. 

Sau đánh giá ban đầu, có một hành động tùy chọn duy nhất được gọi là bác bỏ. Panda được phép lựa chọn nhiều nhất$b$các giấy tờ riêng biệt và áp dụng thao tác bác bỏ này cho chúng. Hoạt động này sẽ thay đổi điểm của mọi người đánh giá trên bài báo đó theo một cách rất có cấu trúc: điểm tích cực giảm đi một, trong khi điểm không tích cực tăng thêm một. Điều này có nghĩa là điểm cao giảm đi một chút và điểm thấp được cải thiện đôi chút, nhưng sự thay đổi không cân xứng về tổng tác động. 

Mục đích là để quyết định nên áp dụng phản bác trên những giấy tờ nào để sau tất cả những thay đổi, số lượng giấy tờ được chấp nhận sẽ được tối đa hóa. 

Cấu trúc ràng buộc chính là mỗi bài báo có tối đa 10 người đánh giá và có tới 200.000 bài báo trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng sự kết hợp của các bài báo đã chọn hoặc tính toán lại các trạng thái toàn cầu một cách lặp đi lặp lại. Bất kỳ giải pháp nào cũng phải xử lý từng bài báo gần như độc lập và giảm vấn đề thành sắp xếp hoặc lựa chọn tham lam trong thời gian tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp xuất hiện khi một bài báo đã được chấp nhận nhưng lại bị từ chối sau khi bác bỏ. Ví dụ: nếu ban đầu một bài báo hầu như không vượt qua ngưỡng nhưng hầu hết người đánh giá đều cho điểm tích cực thì việc áp dụng thao tác có thể làm giảm tổng số điểm đến mức hạ nó xuống dưới$k$. Một chiến lược ngây thơ “bác bỏ mọi thứ có thể giúp ích” sẽ thất bại ở đây. 

Một trường hợp đặc biệt khác là khi một bài báo ban đầu bị từ chối nhưng lại được chấp nhận sau khi bị bác bỏ. Đây là tình huống duy nhất mà việc áp dụng thao tác này là hữu ích và việc xác định chính xác thời điểm điều này xảy ra là cốt lõi của vấn đề. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực trực tiếp sẽ thử tối đa mọi tập hợp con$b$giấy tờ và mô phỏng tác dụng của việc áp dụng phản bác. Đối với mỗi tập hợp con, chúng tôi sẽ tính toán lại tất cả các điểm bài thi và đếm xem có bao nhiêu điểm đạt ngưỡng. Điều này đòi hỏi phải đánh giá$\sum_{i=0}^{b} \binom{n}{i}$những khả năng lớn về mặt thiên văn ngay cả đối với những khả năng nhỏ$b$và mỗi đánh giá sẽ yêu cầu$O(nm)$làm việc để tính lại tổng số. Cách tiếp cận này thất bại ngay lập tức ngoài những đầu vào nhỏ. 

Quan sát quan trọng là tác động của việc bác bỏ trên một bài báo là hoàn toàn độc lập với tất cả các bài báo khác. Không có gì kết hợp các quyết định giữa các giấy tờ ngoại trừ ràng buộc toàn cầu mà nhiều nhất$b$giấy tờ có thể được lựa chọn. Điều này làm giảm vấn đề từ tối ưu hóa toàn cục tổ hợp thành vấn đề lựa chọn mức tăng cho mỗi mục. 

Đối với mỗi bài báo, chúng ta chỉ cần so sánh hai trạng thái: trạng thái chấp nhận hiện tại và trạng thái sau khi áp dụng phép biến đổi cố định. Nếu việc áp dụng phản bác làm thay đổi một bài báo từ bị từ chối thành được chấp nhận thì điều đó có lợi. Nếu nó thay đổi từ chấp nhận sang từ chối thì có hại. Nếu nó không thay đổi quyết định, nó không liên quan. 

Khi chúng tôi tính toán mức tăng trên mỗi tờ giấy này, vấn đề toàn cầu nhiều nhất sẽ trở thành việc lựa chọn$b$mặt hàng có mức tăng tích cực. Vì mỗi bài báo đóng góp 0 hoặc 1 cải thiện cho mục tiêu (hoặc tiêu cực, mà chúng tôi sẽ không bao giờ chọn), chiến lược tối ưu chỉ đơn giản là lấy càng nhiều giấy tờ có lợi càng tốt.$b$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong n | O(n) | Quá chậm | 
| Tham lam tối ưu | O(nm) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán tác động của việc áp dụng phản bác trên từng bài báo một cách độc lập và sau đó tham lam chọn ra những ứng viên tốt nhất. 

1. Đối với mỗi bài viết, hãy tính tổng số điểm ban đầu bằng cách cộng tất cả các giá trị của người đánh giá. Điều này xác định liệu nó hiện có được chấp nhận hay không. 
2. Đối với cùng một bài viết, hãy tính xem ý kiến ​​phản bác sẽ thay đổi điểm số như thế nào. Mỗi người đánh giá đóng góp +1 nếu điểm của họ ≤ 0 và −1 nếu điểm của họ ≥ 1. Tổng các đóng góp này sẽ cho ra tổng số thay đổi về điểm cho bài viết đó. 
3. Kết hợp điểm ban đầu và sự thay đổi để có được điểm cuối cùng sau khi bác bỏ và xác định xem bài viết có được chấp nhận ở trạng thái đó hay không. 
4. So sánh trạng thái chấp nhận trước và sau khi bác bỏ. Nếu bài viết thay đổi từ bị từ chối thành được chấp nhận, hãy đánh dấu nó là có lợi với mức tăng +1. Nếu nó thay đổi từ được chấp nhận thành bị từ chối, hãy đánh dấu nó là −1, mặc dù những bài như vậy sẽ không bao giờ được chọn. Nếu nó giữ nguyên thì mức tăng là 0. 
5. Đếm xem có bao nhiêu bài được +1. Vì mỗi lựa chọn là độc lập và hạn chế duy nhất là chọn nhiều nhất$b$giấy tờ, chọn tối đa$b$của những giấy tờ có lợi này. 
6. Đáp án cuối cùng là số lượng giấy tờ được chấp nhận ban đầu cộng với số lượng giấy tờ có lợi được chọn. 

### Tại sao nó hoạt động 

Mỗi tờ giấy hoạt động giống như một mục nhị phân có giá trị chỉ có thể cải thiện, xấu đi hoặc không thay đổi trong một thao tác duy nhất. Bởi vì mục tiêu hoàn toàn mang tính bổ sung giữa các giấy tờ và số lượng hoạt động bị hạn chế trên toàn cầu nhưng không tương tác, nên việc ưu tiên một giấy tờ có lợi hơn một giấy tờ khác sẽ không có lợi ích gì. Mỗi bài viết hữu ích đóng góp chính xác một đơn vị cải tiến trong số lượng cuối cùng, vì vậy bất kỳ tập hợp con nào có kích thước tối đa$b$giữa chúng là tương đương. Điều này làm cho lựa chọn tham lam trở nên tối ưu và ngăn ngừa bất kỳ sự phụ thuộc tiềm ẩn nào giữa các lựa chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, m, k, b = map(int, input().split())
        
        base_accepted = 0
        good = 0
        
        for _ in range(n):
            arr = list(map(int, input().split()))
            
            s = sum(arr)
            base_ok = (s >= k)
            
            delta = 0
            for x in arr:
                if x >= 1:
                    delta -= 1
                else:
                    delta += 1
            
            new_ok = (s + delta >= k)
            
            if base_ok:
                base_accepted += 1
            
            if (not base_ok) and new_ok:
                good += 1
        
        out.append(str(base_accepted + min(b, good)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Đối với mỗi bài báo, chúng tôi tính toán cả số tiền ban đầu và số tiền thay đổi ròng do phản bác gây ra. Vòng lặp qua người đánh giá là nhỏ vì$m \le 10$, vì vậy bước này thực chất là thời gian không đổi trên mỗi tờ giấy. 

Chi tiết triển khai chính là ngầm tách các giấy tờ thành ba loại: đã được chấp nhận, mới được chấp nhận sau khi bác bỏ và mọi thứ khác. Chỉ có quá trình chuyển đổi từ bị từ chối sang được chấp nhận mới góp phần vào số lượng cải thiện cuối cùng. Chúng tôi không bao giờ lưu trữ lợi nhuận một cách rõ ràng vì chỉ số lượng lợi nhuận tích cực mới quan trọng. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với ba tờ giấy. 

Bài A đã có tổng điểm trên ngưỡng rồi nên bước đầu được chấp nhận. Sau khi áp dụng phản bác, giả sử điểm của nó giảm xuống dưới ngưỡng. Giấy A không đóng góp gì cho quỹ có lợi. 

Giấy B ban đầu bị từ chối nhưng được chấp nhận sau khi chuyển đổi. Giấy C vẫn bị từ chối trong cả hai trường hợp. 

Chúng tôi theo dõi việc phân loại: 

| Giấy | Tổng ban đầu | Ban đầu OK | Đồng bằng | Tổng Mới | Mới được | Đạt được | 
| --- | --- | --- | --- | --- | --- | --- | 
| A | cao | 1 | −2 | thấp hơn | 0 | −1 | 
| B | trung bình | 0 | +3 | cao | 1 | +1 | 
| C | thấp | 0 | +1 | vẫn còn thấp | 0 | 0 | 

Nếu như$b = 1$, chúng tôi chỉ chọn Bài B, chấp nhận thêm một bài. Nếu như$b = 2$, việc chọn nhiều hơn không giúp ích gì vì chỉ có một bài báo có mức tăng dương. 

Điều này chứng tỏ rằng quyết định hoàn toàn chỉ dừng lại ở việc đếm xem có bao nhiêu bài bị từ chối có thể được chuyển thành những bài được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi bài báo yêu cầu tối đa 10 người đánh giá phải vượt qua một lần | 
| Không gian |$O(1)$thêm | Chỉ có quầy được duy trì | 

Các ràng buộc cho phép lên đến$2 \times 10^5$về tổng thể, và vì mỗi bài chỉ yêu cầu công việc liên tục nên giải pháp dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    t = int(input())
    res = []
    
    for _ in range(t):
        n, m, k, b = map(int, input().split())
        base = 0
        good = 0
        
        for _ in range(n):
            arr = list(map(int, input().split()))
            s = sum(arr)
            ok = s >= k
            
            delta = sum(1 if x <= 0 else -1 for x in arr)
            new_ok = (s + delta >= k)
            
            if ok:
                base += 1
            if (not ok) and new_ok:
                good += 1
        
        res.append(str(base + min(b, good)))
    
    return "\n".join(res)

# minimal case
assert run("""1
1 1 0 1
0
""") == "1"

# simple flip case
assert run("""1
1 1 1 1
0
""") == "1"

# no benefit case
assert run("""1
2 1 5 1
3
3
""") == "0"

# mixed case
assert run("""1
3 2 1 2
1 1
-3 -3
2 -1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 1 | chấp nhận giấy đơn | 
| ốp lưng đơn giản | 1 | phản bác có thể chuyển đổi từ chối | 
| trường hợp không có lợi ích | 0 | không có giấy đạt ngưỡng | 
| trường hợp hỗn hợp | 2 | hạn chế lựa chọn và lợi ích | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi một bài báo đã được chấp nhận nhưng lại bị từ chối sau khi áp dụng phản bác. Trong trường hợp này, mức tăng là âm và thuật toán tự nhiên bỏ qua nó vì nó chỉ tính các chuyển đổi từ chối sang chấp nhận. Ví dụ: nếu một tờ giấy có tổng chính xác bằng$k$và hầu hết những người đánh giá đều cho điểm tích cực, delta có thể đẩy nó xuống dưới ngưỡng, nhưng vì nó không bao giờ xuất hiện trong nhóm có lợi nên nó không bao giờ được chọn. 

Một trường hợp khác xảy ra khi không có giấy nào được cải thiện sau thao tác. Trong trường hợp đó, số lượng giấy tờ có lợi bằng 0 và câu trả lời đúng sẽ giảm xuống số lượng giấy tờ được chấp nhận ban đầu.
