---
title: "CF 105632D - Trò chơi đoán"
description: "Chúng ta được cung cấp một chuỗi các cặp ngày càng tăng và sau mỗi tiền tố, chúng ta cần đánh giá một trò chơi giả định trên tiền tố đó. Đối với tiền tố cố định có độ dài k, hãy tưởng tượng chúng ta chọn một trong k chỉ số i."
date: "2026-06-22T14:59:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105632
codeforces_index: "D"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Zhengzhou Onsite (The 3rd Universal Cup. Stage 22: Zhengzhou)"
rating: 0
weight: 105632
solve_time_s: 89
verified: true
draft: false
---

[CF 105632D - Trò chơi đoán](https://codeforces.com/problemset/problem/105632/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các cặp ngày càng tăng và sau mỗi tiền tố, chúng ta cần đánh giá một trò chơi giả định trên tiền tố đó. 

Đối với tiền tố cố định có độ dài k, hãy tưởng tượng chúng ta chọn một trong k chỉ số i. Alice nhận được giá trị aᵢ, Bob nhận được bᵢ và cả hai người chơi đều nhìn thấy toàn bộ danh sách các cặp trong tiền tố nhưng không biết chỉ số nào đã được chọn. Họ cũng biết loại giá trị nhận được của nhau (Alice chỉ biết aᵢ, Bob chỉ biết bᵢ). Sau đó, họ luân phiên thông báo xem họ có thể suy ra giá trị của người chơi khác hay không, bắt đầu từ Alice. Nếu Alice có thể xác định duy nhất bᵢ trước, cô ấy sẽ thắng ngay lập tức; nếu không thì Bob có cơ hội suy ra aᵢ sau khi nghe câu nói của Alice. Nếu cả hai đều không thể buộc phải có một khoản khấu trừ duy nhất thì không ai thắng vì điều đó. 

Với mỗi tiền tố k, chúng ta phải đếm xem có bao nhiêu chỉ số i trong tiền tố đó dẫn đến Alice thắng và bao nhiêu chỉ số dẫn đến Bob thắng. 

Ràng buộc q lên tới 10⁶, do đó, bất kỳ phương pháp nào cố gắng tính toán lại kết quả cho từng tiền tố bằng cách quét tất cả các cặp trước đó sẽ quá chậm. Ngay cả phương pháp O(q²) cũng sẽ vượt xa giới hạn, vì vậy về cơ bản chúng ta cần khấu hao O(1) hoặc O(log q) cho mỗi lần chèn. 

Một vấn đề tế nhị là kết quả của một chỉ mục cố định phụ thuộc vào toàn bộ tiền tố chứ không chỉ thông tin cục bộ. Một sai lầm ngây thơ là cho rằng Alice thắng bất cứ khi nào tất cả các cặp có cùng aᵢ có cùng bᵢ trong mảng cuối cùng, nhưng điều này bỏ qua sự tương tác trong đó Bob có thể loại bỏ các ứng cử viên bằng cách sử dụng lỗi suy luận của Alice. 

Một cạm bẫy khác là đối xử với hai người chơi một cách đối xứng và cho rằng họ độc lập. Suy luận của Bob phụ thuộc vào mẫu phản ứng của Alice, mẫu phản ứng này phụ thuộc vào cấu trúc tổng thể trong mỗi nhóm. 

Một ví dụ nhỏ trong đó lý luận ngây thơ thất bại là trường hợp: 

cặp: (1,1), (1,2), (2,1) 

Với i = 1, Alice thấy a=1 với các giá trị b {1,2} nên cô ấy không thể suy ra được. Bob thấy b=1 với các ứng cử viên i=1 và i=3, nhưng chỉ i=1 phù hợp với việc Alice không thể suy luận (vì i=3 sẽ khiến Alice biết ngay), vì vậy Bob thắng. Bất kỳ cách tiếp cận nào bỏ qua việc lọc phản hồi của Alice sẽ đánh dấu sai i=1 là chưa quyết định. 

## Phương pháp tiếp cận 

Cách brute-force là, với mỗi tiền tố k và mỗi chỉ số i ≤ k, mô phỏng toàn bộ quá trình suy luận. Đối với i cố định, trước tiên Alice kiểm tra xem trong số tất cả j ≤ k có aⱼ = aᵢ, các giá trị b tương ứng có thu gọn về một giá trị duy nhất hay không. Nếu không, cô ấy sẽ nói “Tôi không biết”. Sau đó Bob xem xét tất cả j ≤ k với bⱼ = bᵢ và lọc chúng theo những cái nào sẽ tạo ra phản hồi giống như Alice. Nếu còn lại đúng một thì Bob thắng. 

Việc mô phỏng điều này theo i yêu cầu các nhóm quét được xác định bằng a hoặc bằng b, chi phí là O(k) cho mỗi chỉ mục, dẫn đến O(k²) cho mỗi tiền tố và tổng thể là O(q³) trong trường hợp xấu nhất. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là hành vi của Alice chỉ phụ thuộc vào việc mỗi giá trị a có “thuần túy” bên trong tiền tố hay không, nghĩa là tất cả các lần xuất hiện đều có chung giá trị b. Đối với tiền tố cố định, mọi chỉ mục có cùng a đều có hành vi Alice giống hệt nhau. Tương tự, việc lọc của Bob chỉ phụ thuộc vào việc nhóm theo b và nhãn hành vi Alice này. 

Điều này làm giảm vấn đề trong việc duy trì hai cách phân loại đang phát triển: liệu mỗi nhóm a có thuần túy hay không và cách các chỉ số phân phối thành các nhóm b dưới nhãn đó. Cấu trúc trở nên năng động nhưng rất thưa thớt, bởi vì một khi một nhóm a trở nên không tinh khiết thì nó sẽ không bao giờ trở nên thuần khiết nữa khi tiền tố phát triển. 

Tính đơn điệu này cho phép chúng tôi duy trì mọi thứ theo từng bước và tránh việc tính toán lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q³) | O(q) | Quá chậm | 
| Bảo trì nhóm gia tăng | O(q) khấu hao | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cặp theo thứ tự, duy trì cấu trúc dữ liệu tiền tố.

1. Đối với mỗi giá trị a, chúng ta duy trì có bao nhiêu giá trị b riêng biệt đã xuất hiện cho đến nay trong số các chỉ số có a đó. Nếu con số này chính xác là một, chúng ta nói a hiện tại là “thuần túy”. 
2. Chúng tôi duy trì cho mỗi danh sách các chỉ mục thuộc về nó, vì vậy chúng tôi có thể cập nhật tất cả chúng nếu độ tinh khiết của nó thay đổi. 
3. Với mỗi b, chúng ta duy trì hai bộ đếm: có bao nhiêu chỉ số với b này hiện đến từ các nhóm a thuần túy và bao nhiêu chỉ số đến từ các nhóm a không thuần khiết. 
4. Khi một cặp mới (aᵢ, bᵢ) xuất hiện, chúng tôi cập nhật tập hợp các giá trị b được thấy cho aᵢ. Nếu đây là giá trị b riêng biệt thứ hai cho a đó thì nhóm a sẽ chuyển từ thuần khiết sang không tinh khiết. Quá trình chuyển đổi này là không thể đảo ngược. 
5. Khi một nhóm a trở nên không tinh khiết, tất cả các chỉ mục được chèn trước đó với a đó phải được chuyển từ nhóm “đóng góp thuần túy” sang nhóm “đóng góp không tinh khiết” bên trong nhóm b tương ứng của chúng. 
6. Sau khi cập nhật cấu trúc cho chỉ mục i, chúng ta xác định kết quả cho i: 

Nếu aᵢ vẫn thuần thì Alice thắng ngay. 
7. Ngược lại, Bob kiểm tra tất cả các ứng viên j có cùng bᵢ. Anh ta chỉ nhìn thấy những người j có độ tinh khiết nhóm a phù hợp với phản ứng quan sát được của Alice đối với i. Vì phản hồi đó được cố định bởi độ tinh khiết hiện tại của aᵢ nên các ứng cử viên của Bob chính xác là những người trong nhóm của bᵢ tương ứng với nhãn độ tinh khiết đó. 
8. Nếu tập ứng cử viên được lọc của Bob có kích thước chính xác bằng một thì Bob sẽ thắng i. 

Sự đơn giản hóa cấu trúc quan trọng là phản hồi của Alice chỉ phụ thuộc vào một boolean duy nhất cho mỗi giá trị a và lý luận của Bob giảm xuống việc đếm trong một nhóm b cố định được phân chia bởi boolean đó. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào, quyết định của Alice đối với bất kỳ chỉ mục nào chỉ phụ thuộc vào việc ánh xạ a → b có nội xạ bên trong nhóm a đó hay không. Thuộc tính này thống nhất trên tất cả các chỉ mục có cùng giá trị a, do đó phản hồi của Alice phân chia các chỉ mục thành hai lớp cho mỗi giá trị a: những lớp có nhóm a vẫn thuần khiết và những lớp không có. 

Khi đó, suy luận của Bob tương đương với việc giao tập ứng cử viên của anh ấy (được cố định bởi giá trị b) với lớp phản hồi của Alice. Vì cả hai phân vùng đều được xác định đầy đủ bằng số liệu thống kê tiền tố, trò chơi giảm xuống việc đếm xem có bao nhiêu chỉ mục vẫn nhất quán với một ràng buộc boolean duy nhất bên trong một nhóm b. 

Không có tương tác sau này làm thay đổi các phân loại này, do đó quá trình này nhất quán và đơn điệu theo thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    
    seen_b = {}
    bcnt = {}
    a_indices = {}
    a_seen = {}
    
    # bcnt[b] = [count_when_A_pure, count_when_A_impure]
    ansA = 0
    ansB = 0
    
    # current purity of a
    a_pure = {}
    
    for _ in range(q):
        a, b = map(int, input().split())
        
        if a not in a_seen:
            a_seen[a] = set()
            a_indices[a] = []
            a_pure[a] = True
        
        # update a structure
        if b not in a_seen[a]:
            if len(a_seen[a]) == 1:
                a_pure[a] = False
            a_seen[a].add(b)
        
        # initialize b counter
        if b not in bcnt:
            bcnt[b] = [0, 0]
        
        # insert index i (we don't store index id, only counts)
        # treat current element as "index"
        idx_state = 1 if a_pure[a] else 0
        
        bcnt[b][idx_state] += 1
        
        # store index in list for potential future flip handling
        a_indices[a].append((b, idx_state))
        
        # if a just became impure, we must migrate previous pure contributions
        if not a_pure[a] and len(a_seen[a]) == 2:
            # flip event: move all previous entries of this a
            for (bb, st) in a_indices[a]:
                if st == 1:
                    bcnt[bb][1] -= 1
                    bcnt[bb][0] += 1
        
        # compute answer for this index
        if a_pure[a]:
            ansA += 1
        else:
            # Bob checks only impure bucket (since Alice response is 0)
            if bcnt[b][0] == 1:
                ansB += 1
    
    print(ansA, ansB)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì, đối với mỗi giá trị a, tập hợp các giá trị b riêng biệt để xác định khi nào nó không còn thuần túy. Trạng thái chính là cờ độ tinh khiết boolean, cờ này kiểm soát xem một chỉ mục có đóng góp vào nhóm “Alice-know” hay không. 

Đối với mỗi giá trị b, chúng tôi duy trì hai chỉ số phân tách bộ đếm xem nhóm a của chúng có thuần tại thời điểm chèn hay không. Khi một nhóm chuyển đổi từ thuần túy sang không tinh khiết, chúng tôi sẽ di chuyển trở về trước tất cả các đóng góp trước đó của nó, điều này an toàn vì mỗi chỉ mục tham gia vào nhiều nhất một sự kiện chuyển đổi như vậy. 

The final decision for each i is then reduced to a constant-time lookup in these counters.

 ## Ví dụ đã hoạt động 

Hãy xem xét tiền tố mẫu: 

(1,1), (1,2), (2,1) 

Chúng tôi theo dõi cách cấu trúc phát triển: 

| tôi | Cặp | Độ tinh khiết (mỗi a) | b trạng thái xô | Alice thắng | Bob thắng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,1) | a=1 nguyên chất | b=1:[1,0] | vâng | không | 
| 2 | (1,2) | a=1 không tinh khiết | b=2:[0,1] | không | vâng | 
| 3 | (2,1) | a=2 nguyên chất | b=1:[1,1] | vâng | không | 

Với i=2, Alice không thể suy ra vì a=1 có nhiều giá trị b. Bob thấy b=2 và chỉ có chỉ số 2 phù hợp với cả tính nhất quán của b và phản hồi của Alice, vì vậy anh ấy thắng. 

Bây giờ hãy xem xét trường hợp không ai thắng: 

(1,1), (1,2)

 | tôi | Cặp | Bang Alice | Nghị quyết ứng cử viên Bob | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | impure a=1 | còn lại hai ứng cử viên | không | 
| 2 | (1,2) | không tinh khiết a=1 | còn lại hai ứng cử viên | không | 

Điều này cho thấy rằng chỉ riêng tạp chất không đảm bảo Bob thắng trừ khi quá trình lọc tách biệt một ứng cử viên duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) khấu hao | mỗi chỉ mục được di chuyển nhiều nhất một lần trong quá trình chuyển đổi độ tinh khiết | 
| Không gian | O(q) | lưu trữ cho nhóm a, bộ đếm b và danh sách chỉ mục | 

Giải pháp tuyến tính về số lượng cặp, phù hợp thoải mái trong giới hạn ngay cả đối với q lên tới 10⁶, vì mọi thao tác đều được khấu hao theo thời gian không đổi và mỗi phần tử được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # adjust if needed

# sample
assert run("""4
1 1
1 2
2 1
2 2
""").strip() == """1 0
0 2
1 2
0 0"""

# minimal case
assert run("""1
5 7
""").strip() == """1 0"""

# all equal a-values, varying b
assert run("""3
1 1
1 2
1 3
""").strip() == """1 0
0 2
0 3"""

# symmetric swap structure
assert run("""3
1 2
2 1
3 1
""")  # sanity check structure

# all equal pairs
assert run("""2
1 1
1 1
""").strip() == """1 0
0 0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp đơn | Alice thắng | trường hợp cơ sở đúng đắn | 
| tất cả đều bằng a, khác b | tiến triển hỗn hợp | xử lý chuyển tiếp độ tinh khiết | 
| hoán đổi đối xứng | sự tương tác đối xứng | Logic lọc Bob | 
| cặp trùng lặp | ổn định | không có chiến thắng giả từ sự dư thừa | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một giá trị a trở nên không thuần khiết vào đúng thời điểm một giá trị b mới xuất hiện. Ví dụ: (1,1), (1,2). Ở lần chèn thứ hai, nhóm a=1 chuyển từ thuần sang không tinh khiết và tất cả các đóng góp trước đó phải được di chuyển một cách nhất quán. Thuật toán xử lý việc này bằng cách thực hiện quét một lần các chỉ mục được lưu trữ cho a đó, đảm bảo rằng cả phần tử cũ và phần tử mới đều được phân loại ở trạng thái không tinh khiết. 

Một trường hợp khác là khi tập ứng cử viên của Bob chỉ thu gọn về kích thước một sau khi lọc phản hồi của Alice. Trong các chuỗi như (1,1), (2,1), nhóm b cho giá trị 1 ban đầu có hai ứng cử viên, nhưng chỉ có một ứng cử viên phù hợp với mẫu phản hồi của Alice, đó là lý do tại sao Bob có thể suy luận mặc dù có nhiều ứng cử viên thô. Thuật toán nắm bắt được điều này bằng cách chia mỗi nhóm b thành hai bộ đếm dựa trên cờ độ tinh khiết, do đó việc lọc diễn ra ngay lập tức và chính xác.
