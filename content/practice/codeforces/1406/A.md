---
title: "CF 1406A - Tập hợp con Mex"
description: "Chúng ta được cấp một tập hợp các số nguyên và chúng ta được phép phân phối lại các phần tử của nó thành hai nhóm, gọi chúng là A và B mà không thay đổi tổng số bản sao của mỗi giá trị tồn tại. Mọi phần tử phải thuộc đúng một trong hai nhóm, nhưng các phần tử trùng lặp có thể được phân chia tùy ý."
date: "2026-06-11T07:53:45+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1406
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 670 (Div. 2)"
rating: 900
weight: 1406
solve_time_s: 84
verified: true
draft: false
---

[CF 1406A - Tập hợp con Mex](https://codeforces.com/problemset/problem/1406/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên và chúng ta được phép phân phối lại các phần tử của nó thành hai nhóm, gọi chúng là A và B mà không thay đổi tổng số bản sao của mỗi giá trị tồn tại. Mọi phần tử phải thuộc đúng một trong hai nhóm, nhưng các phần tử trùng lặp có thể được phân chia tùy ý. 

Đối với bất kỳ nhóm nào, chúng tôi tính toán mex, là số nguyên không âm nhỏ nhất không xuất hiện trong nhóm đó. Nhiệm vụ là gán các phần tử vào A và B sao cho tổng mex(A) + mex(B) càng lớn càng tốt. 

Cấu trúc chính là mex chỉ phụ thuộc vào việc chúng ta có thể “che” chuỗi bắt đầu từ 0 hay không. Nếu một số bị thiếu trong một tập hợp, mọi thứ nằm ngoài số đó sẽ trở nên không liên quan đến mex của nó. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm và mỗi mảng có kích thước tối đa là 100. Điều này ngay lập tức loại trừ bất kỳ điều gì ngoài lý luận tuyến tính hoặc bậc hai cho mỗi trường hợp thử nghiệm. Ngay cả O(n³) cũng có thể vượt qua, nhưng chúng tôi không cần bất cứ thứ gì gần với mức đó. Vấn đề là dành cho một đối số đếm tham lam. 

Trường hợp khó nhận biết xuất hiện khi một số số nhỏ bị thiếu hoàn toàn. Nếu thiếu 0, cả hai bộ ngay lập tức có mex 0 bất kể điều gì khác. Một trường hợp khác là khi một số chỉ xuất hiện một lần: nó có thể đóng góp vào khả năng mở rộng tiền tố liên tiếp của nhiều nhất một trong hai bộ, điều này trực tiếp giới hạn mức độ lớn của cả hai giá trị mex có thể đồng thời trở thành. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách để gán từng phần tử cho A hoặc B. Có 2^n phép gán như vậy và đối với mỗi phép tính hai giá trị mex, chi phí là O(n). Giá trị này trở thành O(n·2^n), quá lớn ngay cả với n = 100. 

Điều quan trọng cần lưu ý là mex(A) = k có nghĩa là A phải chứa mọi số từ 0 đến k−1 ít nhất một lần. Điều tương tự cũng áp dụng cho B. Vì vậy, vấn đề trở thành là có bao nhiêu tiền tố hoàn chỉnh bắt đầu từ 0 mà chúng ta có thể “cung cấp” đồng thời cho hai nhóm bằng cách sử dụng số lượng bản sao giới hạn của mỗi số. 

Thay vì suy nghĩ về các bài tập đầy đủ, chúng ta chỉ cần biết mỗi giá trị cung cấp bao nhiêu bản sao có thể sử dụng để xây dựng các tiền tố này. Mỗi số x có thể đóng góp tổng cộng tối đa hai lần vào hai phép tính mex, nhưng chỉ khi nó xuất hiện ít nhất hai lần. Nếu xuất hiện một lần thì chỉ giúp được một bên. Nếu nó xuất hiện 0 lần, nó sẽ ngắt cả hai tiền tố ngay lập tức. 

Điều này làm giảm vấn đề khi quét các giá trị từ 0 trở lên và theo dõi xem liệu chúng ta có thể tiếp tục xây dựng mex thứ nhất và thứ hai cùng một lúc hay không. Mỗi lần chúng ta gặp một giá trị có tần số ít nhất là 2 thì cả A và B đều có thể bao gồm nó. Nếu tần số chính xác bằng 1 thì chỉ một trong số chúng có thể mở rộng, do đó một “chuỗi” tiếp tục trong khi chuỗi kia có thể buộc phải dừng trừ khi chúng ta quản lý nhiệm vụ một cách cẩn thận. Việc theo dõi tham lam này dẫn đến việc tính toán trực tiếp xem cả hai giá trị mex có thể được mở rộng bao xa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tần số quét tham lam | O(maxA_i) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén đầu vào thành một mảng tần số trong đó cnt[x] lưu trữ số lần x xuất hiện. 

Sau đó chúng tôi mô phỏng việc xây dựng song song hai chuỗi mex.

1. Chúng tôi duy trì hai cờ, cho biết liệu A và B vẫn có thể tiếp tục xây dựng các tiền tố liên tiếp bắt đầu từ 0. Ban đầu cả hai đều hoạt động. 
2. Chúng ta lặp x từ 0 trở lên. 
3. Nếu cnt[x] == 0, cả hai chuỗi buộc phải dừng ở x, vì không tập hợp nào có thể chứa x, vì vậy cả hai giá trị mex tối đa đều là x. 
4. Nếu cnt[x] == 1 thì đúng một trong hai bộ có thể nhận được x. Điều này có nghĩa là chỉ một tiền tố có thể tiếp tục đi qua x, trong khi tiền tố kia phải dừng ở x. Chúng tôi chỉ định sự xuất hiện đơn lẻ này một cách tham lam để mở rộng phía nhỏ hơn hoặc hiện ít "bão hòa" hơn, nhưng vì chúng tôi chỉ quan tâm đến độ dài nên nó đóng góp tổng cộng một phần mở rộng cho cả hai phép tính mex. 
5. Nếu cnt[x] >= 2, cả hai bộ có thể bao gồm x, vì vậy cả hai tiền tố đều có thể tiếp tục một cách an toàn. 
6. Chúng tôi tích lũy các khoản đóng góp: mỗi khi một tập hợp bao gồm x thành công, nó sẽ tăng ranh giới mex tiềm năng của nó. 
7. Quá trình dừng lại khi cả hai chuỗi không thể kéo dài được nữa. 

Câu trả lời cuối cùng là tổng quãng đường mà A và B đã đạt được. 

Lý do nó hoạt động bắt nguồn từ thực tế là mex hoàn toàn được xác định bởi tiền tố liên tiếp dài nhất bắt đầu từ 0. Mỗi giá trị x chỉ liên quan nếu tất cả các giá trị nhỏ hơn đã được hỗ trợ. Ở mỗi bước, yếu tố hạn chế duy nhất là liệu chúng ta có đủ bản sao của x để cung cấp cả hai tiền tố hay không. Nếu chúng tôi làm vậy, cả hai đều tiếp tục; nếu chúng ta có một, chỉ có một tiếp tục; nếu chúng ta không có thì cả hai đều kết thúc. Không có giá trị nào sau này có thể sửa chữa được giá trị bị thiếu trước đó, vì vậy quá trình này không thể đảo ngược và các quyết định tham lam là an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        cnt = [0] * 105
        for x in a:
            cnt[x] += 1
        
        ans = 0
        
        for x in range(0, 105):
            if cnt[x] == 0:
                break
            elif cnt[x] == 1:
                ans += 1
            else:
                ans += 2
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ nén nhiều tập hợp vào một bảng tần số và sau đó quét các giá trị theo thứ tự tăng dần. Câu trả lời được tích lũy trực tiếp: một giá trị có hai bản sao trở lên đóng góp hai “công dụng”, nghĩa là cả hai bộ đều có thể đưa giá trị đó vào chuỗi tiền tố của chúng; một giá trị có một bản sao chỉ đóng góp một giá trị; và một giá trị bị thiếu sẽ chấm dứt cả hai chuỗi ngay lập tức vì không tập hợp nào có thể đạt được mex vượt quá điểm đó. 

Chi tiết triển khai quan trọng là điểm ngắt sớm trên cnt[x] == 0. Nếu không có nó, chúng tôi sẽ giả định không chính xác rằng các giá trị sau này có thể bù cho phần tử tiền tố bị thiếu, điều này là không thể theo định nghĩa của mex. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
0 2 1 5 0 1
```Chúng tôi tính toán tần số: 

| x | cnt[x] | đóng góp | 
| --- | --- | --- | 
| 0 | 2 | 2 | 
| 1 | 2 | 2 | 
| 2 | 1 | 1 | 
| 3 | 0 | dừng lại | 

Ta tích lũy cho đến khi x = 2: 

Tổng cộng = 2 + 2 + 1 = 5 

Điều này phù hợp với sự phân chia tối ưu trong đó cả hai bộ có thể tạo tiền tố dài nhưng chỉ tồn tại một bản sao của 2. 

### Ví dụ 2 

đầu vào:```
3
0 1 2
```| x | cnt[x] | đóng góp | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 1 | 1 | 
| 2 | 1 | 1 | 
| 3 | 0 | dừng lại | 

Tổng cộng = 3 

Điều này tương ứng với việc một bộ dừng sớm hơn trong khi bộ kia tiếp tục, nhưng vì mọi giá trị đều là một bản sao duy nhất nên cả hai chuỗi đều cạnh tranh và tổng bị giới hạn bởi các phần tử có sẵn. 

Những dấu vết này cho thấy thuật toán đang đếm một cách hiệu quả số lần chúng ta có thể “trả” cho việc mở rộng đồng thời mex(A) và mex(B) dọc theo trục số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(max Ai) mỗi lần kiểm tra | Chúng tôi quét mảng tần số một lần lên tới 100 | 
| Không gian | O(tối đa Ai) | Lưu trữ mảng tần số | 

Các giá trị giới hạn ràng buộc ở mức 100 và n ở mức 100, do đó quá trình quét tuyến tính này không đáng kể cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        cnt = [0] * 105
        for x in a:
            cnt[x] += 1
        ans = 0
        for x in range(105):
            if cnt[x] == 0:
                break
            ans += min(2, cnt[x])
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""4
6
0 2 1 5 0 1
3
0 1 2
4
0 2 0 1
6
1 2 3 4 5 6
""") == """5
3
4
0"""

# custom cases
assert run("""1
1
0
""") == "1", "single element"

assert run("""1
2
0 0
""") == "2", "duplicate zero"

assert run("""1
5
0 0 1 1 2
""") == "5", "balanced duplicates"

assert run("""1
3
1 2 3
""") == "0", "missing zero stops immediately"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | tiền tố tối thiểu | 
| 0 0 | 2 | xử lý trùng lặp | 
| 0 0 1 1 2 | 5 | đa dạng hỗn hợp | 
| 1 2 3 | 0 | thiếu số 0 phá vỡ cả hai | 

## Vỏ cạnh 

Khi mảng hoàn toàn không chứa 0, cả hai giá trị mex đều bị ép về 0 vì cả A và B đều không thể bắt đầu tạo tiền tố hợp lệ. Thuật toán xử lý việc này thông qua lần lặp đầu tiên trong đó cnt[0] == 0, ngay lập tức phá vỡ và trả về 0. 

Khi một giá trị xuất hiện chính xác một lần, nó sẽ giới hạn tổng mức tăng trưởng kết hợp của cả hai chuỗi mex. Ví dụ: nếu cnt[0] = 1, chỉ một bộ có thể bắt đầu xây dựng tiền tố hợp lệ, do đó tổng đóng góp bị giới hạn ở mức 1. Quá trình quét chỉ thêm chính xác một đơn vị và sau đó chỉ tiếp tục ngầm cho chuỗi hoạt động. 

Khi tất cả các giá trị xuất hiện ít nhất hai lần cho đến một số k, cả hai giá trị mex đều tăng theo từng bước cho đến số bị thiếu đầu tiên. Thuật toán thêm 2 cho mỗi giá trị như vậy, phản ánh rằng cả hai bộ có thể bao gồm số đó một cách độc lập và dừng chính xác khi tiền tố bị hỏng.
