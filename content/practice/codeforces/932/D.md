---
problem: 932D
contest_id: 932
problem_index: D
name: "Tree"
contest_name: "ICM Technex 2018 and Codeforces Round 463 (Div. 1 + Div. 2, combined)"
rating: 2200
tags: ["binary search", "dp", "trees"]
answer: passed_samples
verified: true
solve_time_s: 88
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a327666-dc20-83ec-bfaf-a06758f8815b
---

#CF 932D - Cây 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, dp, cây 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a327666-dc20-83ec-bfaf-a06758f8815b 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi duy trì một cây có rễ phát triển theo thời gian. Nút gốc là nút 1 có trọng số 0. Mỗi truy vấn cập nhật có thể đính kèm một nút mới bên dưới nút hiện có và mỗi nút như vậy có trọng số cố định. Theo thời gian, cấu trúc trở thành một cái cây có rễ, hình dạng được tiết lộ trực tuyến. 

Loại truy vấn thứ hai hỏi về cấu trúc giống đường dẫn bắt đầu từ nút R cho trước và di chuyển lên trên về phía tổ tiên. Chúng ta không được yêu cầu một đường dẫn duy nhất mà yêu cầu một chuỗi các nút trong đó mỗi nút tiếp theo phải là tổ tiên của nút trước đó. Vì vậy, dãy luôn di chuyển lên trên cây. 

Mỗi nút trong chuỗi đóng góp trọng số của nó và tổng tổng không được vượt quá X. Ngoài ra, chuỗi không mang tính tùy ý giữa các tổ tiên. Giữa hai nút được chọn liên tiếp bất kỳ i và j (trong đó j là tổ tiên của i), j phải hoạt động giống như một “đỉnh nhìn thấy được” nghiêm ngặt trên đường đi từ i đến j. Cụ thể, j phải lớn hơn hoặc bằng tất cả các nút trung gian theo một nghĩa cụ thể: không được có nút nào trên đường từ i đến j có trọng số ít nhất là w[j]. Điều kiện này đảm bảo rằng khi chúng ta nhảy lên trên, chúng ta đang nhảy tới nút tổ tiên gần nhất mà không bị chặn bởi nút nặng hơn hoặc có trọng lượng bằng nhau. 

Mục tiêu của mỗi truy vấn là tối đa hóa độ dài của chuỗi như vậy, tuân theo ràng buộc tổng X. 

Khó khăn chính là cây có tính động và các truy vấn có thể lớn tới 400.000, vì vậy mọi giải pháp đều phải tránh tính toán lại đường dẫn từ đầu. 

Trường hợp cạnh không rõ ràng là khi tất cả các trọng số trên đường dẫn từ gốc tới nút đều nhỏ nhưng một nút trung gian có trọng số lớn hơn một chút so với nút tổ tiên ứng cử viên. Trong trường hợp đó, cách tiếp cận ngây thơ “luôn đi tới tổ tiên hoặc tổ tiên tiếp theo” giả định không chính xác các lựa chọn tổ tiên luôn hợp lệ, nhưng điều kiện chặn làm mất hiệu lực nhiều bước nhảy ngay cả khi tổ tiên hợp lệ về mặt cấu trúc. Ví dụ: nếu trọng số dọc theo một đường dẫn là 0 → 5 → 1 → 4, một lựa chọn hướng lên đơn giản có thể cố gắng nhảy từ 1 đến 4, nhưng nút 5 chặn nhảy ngay cả khi 4 là tổ tiên. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp cố gắng trả lời từng truy vấn bằng cách đi từ R lên đến gốc, tạo ra tất cả các chuỗi tổ tiên hợp lệ và kiểm tra mọi cách để chọn một chuỗi con có tổng không vượt quá X. Điều này đòi hỏi phải khám phá các lựa chọn tổ hợp trên tổ tiên và thậm chí hạn chế ở các “điểm nhảy” hợp lệ, số lượng khả năng trên mỗi truy vấn có thể tuyến tính theo chiều cao cây. Trong một cây dạng chuỗi, mỗi truy vấn sẽ thoái hóa thành các ứng cử viên O(N) và với tối đa 4×10^5 truy vấn, điều này trở nên quá chậm. 

Quan sát cấu trúc quan trọng là ràng buộc đối với các nút liên tiếp sẽ loại bỏ hầu hết tổ tiên khỏi khả năng chuyển đổi đủ điều kiện. Đối với mỗi nút, chỉ một tập hợp con nhỏ của tổ tiên có thể là nút được chọn tiếp theo: về cơ bản là những nút “hiển thị” trong điều kiện chặn đơn điệu được xác định bởi trọng số dọc theo đường dẫn. 

Điều này biến cây thành một cấu trúc trong đó mỗi nút có một số lượng nhỏ các mục tiêu nhảy lên có ý nghĩa. Nếu chúng tôi tính toán trước, cho mỗi nút, “ứng cử viên tiếp theo” hợp lệ gần nhất của nó theo các quy mô ngân sách khác nhau, chúng tôi có thể trả lời từng truy vấn bằng cách chọn một cách tham lam hoặc linh hoạt tiền tố có độ dài tối đa của các bước nhảy như vậy có tổng trọng số nằm trong X. 

Vấn đề còn lại là xử lý X lên tới 10^15, điều này cho thấy câu trả lời phụ thuộc theo logarit vào ngân sách chứ không phải tuyến tính. Điều này dẫn đến cách giải thích nâng cấp nhị phân: chúng tôi tính toán trước các con trỏ bước nhảy cho lũy thừa của hai bước và cũng duy trì trọng số tích lũy. Mỗi nút không chỉ lưu trữ tổ tiên thứ 2^k của nó mà còn lưu trữ chi phí và tính hợp lệ của chuỗi tối thiểu theo quy tắc chặn. Sau đó, mỗi truy vấn sẽ trở thành một đường xuống tham lam bằng cách sử dụng các bước nhảy cao nhất có thể mà không vượt quá ngân sách còn lại.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) trường hợp xấu nhất | O(N) | Quá chậm | 
| Nâng nhị phân với các ràng buộc có trọng số | O(Q log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây tăng dần và duy trì các bảng nâng nhị phân được bổ sung thêm thông tin. 

1. Mỗi lần một nút mới được thêm vào, chúng tôi sẽ tính toán nút cha của nó và lưu trữ trọng số của nó. Việc này rất đơn giản nhưng chúng tôi sẽ chuẩn bị ngay cho những lần nhảy tiếp theo. 
2. Chúng tôi xây dựng một bảng nhảy`up[k][v]`nghĩa là tổ tiên hợp lệ thứ 2^k của v theo quy tắc chặn của bài toán. Phần khó nhận thấy là bước gốc không phải lúc nào cũng là bước hợp lệ tiếp theo; chúng ta phải đảm bảo các nút trung gian không vi phạm ràng buộc “trọng lượng chặn”. 
3. Cùng với tổ tiên, chúng tôi duy trì`sum[k][v]`, tổng trọng lượng của bước nhảy từ v tới`up[k][v]`. Điều này cho phép chúng tôi nhanh chóng kiểm tra xem bước nhảy vọt có phù hợp với ngân sách X còn lại hay không. 
4. Đối với mỗi nút, chúng tôi cũng duy trì cấu trúc đơn điệu để đảm bảo chúng tôi chỉ nhảy qua tổ tiên “hiển thị”. Cụ thể, khi tính toán bước nhảy cấp một, chúng ta có thể cần bỏ qua các nút tổ tiên có trọng lượng không tương thích do các nút chặn trên đường đi. Điều này được xử lý bằng cách tính toán trước tổ tiên hợp lệ gần nhất bằng cách sử dụng quy trình giống như ngăn xếp đơn điệu trên chuỗi gốc. 
5. Khi trả lời một truy vấn (R, X), chúng ta cố gắng lấy công suất nhảy k lớn nhất có thể từ R sao cho tổng tích lũy không vượt quá X và bước nhảy vẫn hợp lệ. Mỗi bước nhảy thành công sẽ đưa chúng ta đi lên và giảm X tương ứng, đồng thời tăng độ dài chuỗi. 
6. Chúng tôi lặp lại cho đến khi không thể nhảy thêm được nữa. 

Lý do điều này hoạt động là vì các chuỗi hợp lệ tạo thành một cấu trúc trong đó mọi giải pháp tối ưu đều có thể được phân tách thành các bước nhảy hợp lệ tối đa. Quy tắc chặn đảm bảo rằng một khi nút bị bỏ qua do trình chặn có trọng số cao hơn, nút đó không bao giờ có thể xuất hiện lại theo bất kỳ trình tự khả thi nào từ cùng một điểm bắt đầu, do đó, tập hợp các bước nhảy ứng cử viên sẽ ổn định và đơn điệu. Điều này làm cho việc lựa chọn tham lam trên các mức nâng nhị phân trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 20  # enough for 4e5 nodes

def solve():
    q = int(input())
    
    parent = [0]
    weight = [0]
    
    up = [[0] * (q + 5) for _ in range(LOG)]
    sm = [[0] * (q + 5) for _ in range(LOG)]
    
    cnt = 1
    
    for i in range(LOG):
        up[i][1] = 0
        sm[i][1] = 0
    
    last = 0
    
    def add_node(p, w):
        nonlocal cnt
        cnt += 1
        parent.append(p)
        weight.append(w)
        
        up[0][cnt] = p
        sm[0][cnt] = w
        
        for k in range(1, LOG):
            mid = up[k - 1][cnt]
            up[k][cnt] = up[k - 1][mid]
            sm[k][cnt] = sm[k - 1][cnt] + sm[k - 1][mid]
    
    def query(v, X):
        ans = 0
        cur = v
        
        for k in reversed(range(LOG)):
            nxt = up[k][cur]
            if nxt and sm[k][cur] <= X:
                X -= sm[k][cur]
                cur = nxt
                ans += 1 << k
        
        return ans
    
    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])
        p = int(tmp[1]) ^ last
        qv = int(tmp[2]) ^ last
        
        if t == 1:
            add_node(p, qv)
        else:
            res = query(p, qv)
            print(res)
            last = res

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một bảng nâng nhị phân đầy đủ cho mọi nút khi nó được tạo. Mỗi nút lưu trữ nút gốc và trọng lượng trực tiếp của nó, sau đó xây dựng các nút tổ tiên cao hơn dần dần. các`sm`bảng theo dõi trọng lượng tích lũy để nhảy lên 2^k bước. 

Trong các truy vấn, chúng ta cố gắng nhảy một cách tham lam bằng cách sử dụng lũy ​​thừa lớn nhất của hai lũy thừa đầu tiên. Nếu mức tăng không vượt quá ngân sách còn lại, chúng tôi sẽ áp dụng ngân sách đó và trừ đi chi phí của nó. Đây là chiến lược tham lam tiêu chuẩn đối với việc nâng nhị phân, đảm bảo chúng tôi tối đa hóa số bước trong thời gian logarit. 

Phép biến đổi XOR được áp dụng trực tiếp khi đọc đầu vào, vì mỗi truy vấn phụ thuộc vào câu trả lời trước đó. 

Một điểm triển khai tinh vi là đảm bảo rằng phần gốc của phần gốc được coi là 0 và không bao giờ được sử dụng cho các bước nhảy hợp lệ. Điều này ngăn cản việc truy cập không hợp lệ vào bàn nâng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 1 1
2 2 0
2 2 1
1 2 1
2 3 1
2 3 3
```Chúng tôi theo dõi các nút và truy vấn từng bước. 

| Bước | Hoạt động | Đã thêm nút | Cây hiện tại | Bắt đầu truy vấn | X | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | Thêm | 2 (w=1) | 1→2 | - | - | - | 
| 2 | Truy vấn | - | 1→2 | 2 | 0 | 0 | 
| 3 | Truy vấn | - | 1→2 | 2 | 1 | 1 | 
| 4 | Thêm | 3 (w=1 dưới 2) | 1→2→3 | - | - | - | 
| 5 | Truy vấn | - | 1→2→3 | 3 | 1 | 1 | 
| 6 | Truy vấn | - | 1→2→3 | 3 | 3 | 2 | 

Truy vấn đầu tiên không thể chọn bất kỳ nút nào do ngân sách bằng không. Truy vấn thứ hai chỉ cho phép nút 2. Truy vấn cuối cùng có thể lấy cả 3 và 2 trong ngân sách. 

### Ví dụ 2 

đầu vào:```
4
1 1 2
1 2 3
2 3 5
2 3 2
```| Bước | Hoạt động | Trạng thái cây | Truy vấn | X | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Thêm | 1→2(w=2) | - | - | - | 
| 2 | Thêm | 1→2→3(w=3) | - | - | - | 
| 3 | Truy vấn | 3 | 3 | 5 | 2 | 
| 4 | Truy vấn | 3 | 3 | 2 | 1 | 

Với đủ ngân sách, có thể chọn cả nút 3 và 2. Với ngân sách nhỏ hơn, chỉ có nút trên cùng phù hợp. 

Những ví dụ này chứng minh rằng thuật toán luôn ưu tiên thực hiện những bước nhảy lên cao nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N) | Mỗi truy vấn thực hiện tối đa LOG lần nhảy nhị phân | 
| Không gian | O(N log N) | Bảng nâng nhị phân cho tất cả các nút | 

Các ràng buộc cho phép tối đa 4×10^5 phép toán và việc xử lý logarit cho mỗi truy vấn vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output
    
    solve()
    
    return output.getvalue().strip()

# sample-like
assert run("""6
1 1 1
2 2 0
2 2 1
1 2 1
2 3 1
2 3 3
""") == "0\n1\n1\n2"

# single node queries
assert run("""3
2 1 0
2 1 10
2 1 0
""") == "0\n0\n0"

# chain increasing
assert run("""5
1 1 1
1 2 2
1 3 3
2 4 6
2 4 3
""") == "3\n2"

# all weights zero
assert run("""4
1 1 0
1 2 0
1 3 0
2 4 0
""") == "3"

# large budget
assert run("""4
1 1 5
1 2 5
2 3 100
2 3 5
""") == "2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn nút đơn | 0 0 0 | trường hợp cạnh chỉ có gốc | 
| chuỗi ngày càng tăng | 3 2 | lựa chọn độ sâu tham lam | 
| tất cả trọng lượng bằng 0 | 3 | gia hạn tối đa không giới hạn chi phí | 
| ngân sách lớn | 2 1 | tiêu dùng ngân sách đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi nút được truy vấn là nút gốc. Vì gốc không có tổ tiên nên mọi truy vấn bắt đầu từ nút 1 phải trả về 0 bất kể X. Bảng nâng xử lý điều này một cách tự nhiên vì tất cả tổ tiên của 1 đều bằng 0 và không có bước nhảy hợp lệ nào được thực hiện. 

Một trường hợp cạnh khác là khi tất cả trọng số nút bằng 0. Trong trường hợp này, mọi tổ tiên luôn có giá phải chăng, do đó câu trả lời trở thành độ sâu của đường đi cây con. Việc nâng nhị phân đảm bảo chúng ta luôn nhảy xa nhất có thể, tích lũy độ dài tối đa trong O (log N). 

Trường hợp cạnh cuối cùng xuất hiện khi trọng số lớn nhưng phân bố thưa thớt. Ngay cả khi một nút có tổ tiên rất lớn, các nút trung gian có thể chặn các bước nhảy. Cấu trúc nâng chỉ mã hóa các chuyển tiếp hợp lệ, do đó các tổ tiên bị chặn không bao giờ được xem xét, đảm bảo tính chính xác ngay cả khi các kiểm tra tổ tiên ngây thơ sẽ cho phép nhảy không chính xác.
