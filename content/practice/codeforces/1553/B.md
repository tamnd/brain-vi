---
title: "CF 1553B - Chuỗi đảo ngược"
description: "Chúng ta được cung cấp một chuỗi đại diện cho một dòng ký tự. Một con chip bắt đầu ở một số vị trí đã chọn trong chuỗi này. Sau khi đặt xong, chúng ta liên tục di chuyển nó và mỗi lần ghi lại ký tự ở vị trí hiện tại của con chip."
date: "2026-06-18T18:50:09+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "hashing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1553
codeforces_index: "B"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2021-2022 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 1300
weight: 1553
solve_time_s: 94
verified: false
draft: false
---

[CF 1553B - Chuỗi đảo ngược](https://codeforces.com/problemset/problem/1553/B) 

**Đánh giá:** 1300 
**Tags:** brute Force, dp, băm, triển khai, chuỗi 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho một dòng ký tự. Một con chip bắt đầu ở một số vị trí đã chọn trong chuỗi này. Sau khi đặt xong, chúng ta liên tục di chuyển nó và mỗi lần ghi lại ký tự ở vị trí hiện tại của con chip. Chuyển động có cấu trúc chặt chẽ: đầu tiên con chip chỉ di chuyển sang phải trong một số bước, sau đó nó có thể chuyển hướng và chỉ di chuyển sang trái trong một số bước. Nó không bao giờ quay trở lại chuyển động sang phải nữa. 

Điều này có nghĩa là trình tự đã ghi luôn được tạo bằng cách đi dọc theo một đoạn liền kề của chuỗi, đầu tiên là tăng chỉ số và sau đó có thể giảm chỉ số, với chính xác một điểm ngoặt nơi hướng đảo chiều. 

Nhiệm vụ là quyết định liệu một chuỗi mục tiêu có thể được tạo ra dưới dạng một chuỗi các ký tự được truy cập cho một số vị trí bắt đầu và một số lựa chọn di chuyển hay không. 

Các ràng buộc đủ nhỏ để có thể chấp nhận được các giải pháp bậc hai hoặc thậm chí bậc ba đối với các trường hợp thử nghiệm riêng lẻ. Tổng độ dài chuỗi trong tất cả các thử nghiệm tối đa là 500, điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa vượt quá khoảng O(n³) trong trường hợp xấu nhất. Ngay cả O(n²) cho mỗi trường hợp thử nghiệm cũng an toàn. 

Một vài tình huống tế nhị có ý nghĩa quan trọng đối với tính đúng đắn. 

Một vấn đề là khi con đường không bao giờ di chuyển sang trái. Ví dụ: nếu chúng ta bắt đầu ở vị trí 3 và chỉ di chuyển sang phải, chuỗi được tạo ra là chuỗi con đơn giản của chuỗi gốc. Một cách tiếp cận ngây thơ cho rằng chúng ta phải luôn quay ít nhất một lần sẽ bác bỏ những trường hợp như vậy một cách không chính xác. 

Một vấn đề khác là đường dẫn không bao giờ di chuyển ngay sau khi đặt. Bắt đầu từ vị trí i và ngay lập tức di chuyển sang trái cũng hợp lệ, tạo ra một chuỗi có chỉ số giảm dần. Ví dụ: với s = "abcde", bắt đầu từ chỉ số 4 và di chuyển sang trái hai lần sẽ tạo ra "edc". Một giải pháp buộc phải thực hiện bước đi đúng đắn ban đầu sẽ bỏ lỡ điều này. 

Cuối cùng, nhiều cách tiếp cận không chính xác cho rằng câu trả lời chỉ phụ thuộc vào việc khớp chuỗi con hoặc chuỗi con đảo ngược, nhưng điều phức tạp chính là chuỗi có thể thay đổi hướng chính xác một lần, điều này cho phép các hình dạng như "abcba" hoặc các phiên bản một phần như "abcdc". 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp về lực lượng vũ phu là thử mọi vị trí bắt đầu và mô phỏng tất cả các kiểu chuyển động có thể có. Từ chỉ mục bắt đầu i, chúng ta có thể chọn số bước để di chuyển sang phải, sau đó bao nhiêu bước để di chuyển sang trái và xây dựng chuỗi kết quả một cách rõ ràng để so sánh với t. Điều này đúng vì nó tuân theo quy trình chính xác như được xác định. Tuy nhiên, đối với mỗi vị trí bắt đầu, có các lựa chọn O(n²) về độ dài chuyển động phải và trái và việc xây dựng mỗi chuỗi kết quả có chi phí O(n), dẫn đến O(n³) mỗi lần bắt đầu và O(n⁴) tổng thể ở dạng khái niệm tồi tệ nhất. Mặc dù n nhỏ nhưng điều này không cần thiết và gây lãng phí cấu trúc. 

Quan sát quan trọng là chúng ta không bao giờ cần phải quyết định riêng rẽ xem mình sẽ di chuyển sang trái hay sang phải bao xa. Khi chúng tôi cố định vị trí bắt đầu, đường dẫn được mô tả đầy đủ bằng một chuỗi di chuyển dọc theo các chỉ số với nhiều nhất một lần thay đổi hướng. Đây là bước đi bị ràng buộc trên biểu đồ đường, gợi ý một công thức lập trình động theo vị trí trong chuỗi và vị trí trong mục tiêu. 

Thay vì liệt kê độ dài chuyển động, chúng tôi theo dõi xem hiện tại chúng tôi đang di chuyển sang phải hay trái và đảm bảo rằng hướng chỉ có thể chuyển đổi tối đa một lần từ phải sang trái. Điều này làm giảm vấn đề kiểm tra xem liệu chúng ta có thể căn chỉnh t với một đường dẫn trong biểu đồ trạng thái nhỏ có kích thước O(n) hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của tất cả các động tác | O(n⁴) ý tưởng tệ nhất | O(n) | Quá chậm | 
| DP qua vị trí, chỉ số, hướng | O(n³) hoặc O(n²) được tối ưu hóa | O(n2) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi lập mô hình quy trình dưới dạng duyệt qua các chỉ mục trong chuỗi trong khi khớp các ký tự của chuỗi đích. 

1. Chúng tôi thử mọi chỉ mục bắt đầu có thể có trong chuỗi làm vị trí chip ban đầu. Điều này là cần thiết vì quá trình có thể bắt đầu ở bất cứ đâu và ký tự đầu tiên của t phải khớp với vị trí bắt đầu đã chọn. 
2. Từ vị trí bắt đầu cố định, chúng tôi cố gắng khớp chuỗi mục tiêu bằng cách sử dụng trạng thái bao gồm chỉ mục hiện tại trong chuỗi, chỉ mục trong t và hướng chuyển động hiện tại. Hướng có thể là “di chuyển sang phải” hoặc “di chuyển sang trái”. 
3. Lúc đầu, chúng ta không bị ép vào một hướng nào đó nên chúng ta phân thành hai khả năng: nước đi đầu tiên đi sang phải hoặc nước đi đầu tiên đi sang trái. Điều này ghi lại cả hai trường hợp khi bước đi ngay lập tức rẽ hoặc tiếp tục đi theo một hướng trong một thời gian. 
4. Nếu chúng ta đang ở trạng thái “di chuyển sang phải”, chúng ta có thể chuyển từ vị trí i sang i+1, sử dụng ký tự tiếp theo của t. Chúng tôi cũng cho phép một lần chuyển đổi duy nhất từ ​​“di chuyển sang phải” sang “di chuyển sang trái” bất kỳ lúc nào, thể hiện bước ngoặt của bước đi. 
5. Nếu chúng ta đang ở trạng thái “di chuyển sang trái”, chúng ta có thể di chuyển từ vị trí i sang i-1, một lần nữa sử dụng một ký tự của t. Chúng tôi không cho phép chuyển ngược lại sang phải vì quá trình này chỉ cho phép thay đổi một hướng. 
6. Chúng tôi tiếp tục quá trình này cho đến khi chúng tôi sử dụng tất cả các ký tự của t hoặc tất cả các trạng thái có thể có đã hết. Nếu bất kỳ đường dẫn nào tiêu thụ chính xác t thì câu trả lời là có. 

Ràng buộc chính được thực thi bởi máy trạng thái là hướng thay đổi nhiều nhất một lần và chỉ từ phải sang trái. 

### Tại sao nó hoạt động 

Mọi cấu trúc hợp lệ của chuyển động của chip đều tương ứng chính xác với một bước đi bắt đầu tại một số chỉ mục và tuân theo một chuỗi các cạnh trong biểu đồ chuỗi, đầu tiên là tăng chỉ số và sau đó tùy ý giảm nó. DP của chúng tôi liệt kê tất cả các bước đi như vậy mà không trùng lặp bằng cách chỉ mã hóa vị trí hiện tại, tiến trình trong t và giai đoạn định hướng. Vì mọi quá trình chuyển đổi đều phản ánh một chuyển động hợp lệ của chip và chúng tôi không bao giờ cho phép các thay đổi hướng không hợp lệ nên mọi trạng thái được chấp nhận sẽ xây dựng lại một quy trình hợp lệ và bất kỳ quy trình hợp lệ nào đều có thể được DP tuân theo vì nó không bao giờ loại trừ các chuyển đổi hợp pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s, t):
    n, m = len(s), len(t)
    
    for start in range(n):
        if s[start] != t[0]:
            continue
        
        # dp[pos][dir][idx] is too large explicitly,
        # we compress by iterative BFS-style DP over states
        # state: (pos, idx, dir)
        # dir: 0 = right, 1 = left
        
        from collections import deque
        
        vis = set()
        q = deque()
        
        # initial state: at start, matched first character
        # we can choose next direction as right or left
        q.append((start, 0, 0))  # go right
        q.append((start, 0, 1))  # go left
        vis.add((start, 0, 0))
        vis.add((start, 0, 1))
        
        while q:
            pos, i, d = q.popleft()
            
            if i == m - 1:
                return True
            
            if d == 0:
                # move right
                if pos + 1 < n and s[pos + 1] == t[i + 1]:
                    nxt = (pos + 1, i + 1, 0)
                    if nxt not in vis:
                        vis.add(nxt)
                        q.append(nxt)
                # switch to left
                if pos - 1 >= 0 and s[pos - 1] == t[i + 1]:
                    nxt = (pos - 1, i + 1, 1)
                    if nxt not in vis:
                        vis.add(nxt)
                        q.append(nxt)
            else:
                # moving left only
                if pos - 1 >= 0 and s[pos - 1] == t[i + 1]:
                    nxt = (pos - 1, i + 1, 1)
                    if nxt not in vis:
                        vis.add(nxt)
                        q.append(nxt)
    
    return False

def main():
    q = int(input())
    for _ in range(q):
        s = input().strip()
        t = input().strip()
        print("YES" if solve_case(s, t) else "NO")

if __name__ == "__main__":
    main()
```Giải pháp lặp lại trên tất cả các vị trí bắt đầu và khởi chạy một tìm kiếm trạng thái nhỏ từ mỗi vị trí. Trạng thái BFS mã hóa vị trí hiện tại trong chuỗi, tiến trình trong chuỗi đích và liệu chúng ta hiện đang di chuyển sang phải hay sang trái. Logic chuyển đổi phản ánh trực tiếp các chuyển động được phép của chip: từ trạng thái di chuyển sang phải, chúng ta có thể tiếp tục sang phải hoặc chuyển một lần sang trái, trong khi từ trạng thái di chuyển sang trái, chúng ta chỉ có thể tiếp tục sang trái. 

Tập đã truy cập rất quan trọng vì có thể đạt được cùng một cấu hình thông qua các lịch sử chuyển động khác nhau và việc xem lại nó sẽ gây ra hiện tượng nổ tung theo cấp số nhân. Vì không gian trạng thái tối đa là n × m × 2 mỗi lần bắt đầu, nên điều này vẫn hiệu quả theo các ràng buộc đã cho. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

s = "abcdef", t = "cdedcb" 

Chúng tôi thử bắt đầu ở vị trí 2 ('c'). 

| bước | vị trí | chỉ số t | hướng | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | đúng | bắt đầu từ 'c' | 
| 1 | 3 | 1 | đúng | chuyển sang phải tới 'd' | 
| 2 | 4 | 2 | đúng | chuyển sang phải tới 'e' | 
| 3 | 3 | 3 | trái | chuyển và di chuyển sang trái tới 'd' | 
| 4 | 2 | 4 | trái | di chuyển sang trái tới 'c' | 
| 5 | 1 | 5 | trái | di chuyển sang trái tới 'b' | 

Điều này khớp chính xác với mục tiêu, hiển thị quỹ đạo từ phải sang trái hợp lệ. 

### Ví dụ 2 

s = "ab", t = "b" 

Chúng tôi bắt đầu ở vị trí 1 ('b'). 

| bước | vị trí | chỉ số t | hướng | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | trái | bắt đầu từ 'b', không cần di chuyển | 

Điều này chứng tỏ rằng các trường hợp không di chuyển là hợp lệ và phải được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) trường hợp xấu nhất trên mỗi giải pháp đầy đủ, thực tế nhỏ hơn nhiều trong thực tế | Mỗi lần bắt đầu khám phá tối đa các trạng thái O(n × m × 2) và tổng n nhỏ | 
| Không gian | O(n×m) | Bộ nhớ trạng thái đã truy cập mỗi lần bắt đầu ngăn chặn việc tính toán lại | 

Các ràng buộc đảm bảo tổng n trong tất cả các thử nghiệm tối đa là 500, do đó, ngay cả việc khám phá khối cũng vẫn nằm trong giới hạn. Việc cắt bớt trạng thái BFS đảm bảo thời gian chạy thực tế gần với hành vi bậc hai hơn trong các trường hợp điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        q = int(input())
        out = []
        for _ in range(q):
            s = input().strip()
            t = input().strip()
            
            n, m = len(s), len(t)
            from collections import deque
            
            def ok():
                for start in range(n):
                    if s[start] != t[0]:
                        continue
                    
                    qd = deque()
                    vis = set()
                    qd.append((start, 0, 0))
                    qd.append((start, 0, 1))
                    vis.add((start, 0, 0))
                    vis.add((start, 0, 1))
                    
                    while qd:
                        pos, i, d = qd.popleft()
                        if i == m - 1:
                            return True
                        if d == 0:
                            if pos + 1 < n and s[pos+1] == t[i+1]:
                                st = (pos+1, i+1, 0)
                                if st not in vis:
                                    vis.add(st)
                                    qd.append(st)
                            if pos - 1 >= 0 and s[pos-1] == t[i+1]:
                                st = (pos-1, i+1, 1)
                                if st not in vis:
                                    vis.add(st)
                                    qd.append(st)
                        else:
                            if pos - 1 >= 0 and s[pos-1] == t[i+1]:
                                st = (pos-1, i+1, 1)
                                if st not in vis:
                                    vis.add(st)
                                    qd.append(st)
                    return False
            
            out.append("YES" if ok() else "NO")
        return "\n".join(out)
    
    return solve()

# provided samples
assert run("""6
abcdef
cdedcb
aaa
aaaaa
aab
baaa
ab
b
abcdef
abcdef
ba
baa
""") == """YES
YES
NO
YES
YES
NO"""

# custom cases
assert run("""1
a
a
""") == "YES", "single char"

assert run("""1
abc
abc
""") == "YES", "no movement needed"

assert run("""1
abc
cba
""") == "YES", "full reverse via left moves"

assert run("""1
abc
acb
""") == "NO", "impossible zigzag"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | CÓ | trường hợp tối thiểu | 
| abc → abc | CÓ | độ chính xác chuyển động bằng không | 
| abc → cba | CÓ | di chuyển sang trái hoàn toàn | 
| abc → acb | KHÔNG | mẫu hướng không hợp lệ | 

## Vỏ cạnh 

Một chuỗi có độ dài tối thiểu luôn được chấp nhận khi t có cùng một ký tự. DP bắt đầu ở vị trí duy nhất, khớp với ký tự đầu tiên và ngay lập tức kết thúc thành công. 

Trường hợp t bằng s kiểm tra tình huống không cần chuyển động. Thuật toán cho phép chuyển đổi bằng 0 vì trạng thái ban đầu đã khớp với ký tự đầu tiên và có thể kết thúc mà không cần chuyển động bắt buộc. 

Một chuỗi đảo ngược hoàn toàn kiểm tra trường hợp cực đoan khi chúng ta chỉ di chuyển sang trái sau khi sắp xếp. Bắt đầu từ ký tự cuối cùng và di chuyển sang trái từng bước khớp với tất cả các ký tự và DP cho phép hướng trái ban đầu một cách chính xác. 

Mô hình “ngoằn ngoèo” như di chuyển sang phải và sau đó yêu cầu một công tắc độc lập thứ hai quay lại bên phải sẽ bị từ chối vì máy trạng thái cấm thay đổi nhiều hướng, ngăn chặn việc tái tạo không hợp lệ.
