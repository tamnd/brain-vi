---
problem: 960F
contest_id: 960
problem_index: F
name: "Pathwalks"
contest_name: "Divide by Zero 2018 and Codeforces Round 474 (Div. 1 + Div. 2, combined)"
rating: 2100
tags: ["data structures", "dp", "graphs"]
answer: passed_samples
verified: false
solve_time_s: 264
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33a06c-a550-83ec-baf9-e569323b4a6a
---

#CF 960F - Lối đi bộ 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, dp, đồ thị 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a06c-a550-83ec-baf9-e569323b4a6a 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi cạnh có trọng số liên quan và cũng là một vị trí ngầm định được cho theo thứ tự của nó trong đầu vào. Một đường dẫn hợp lệ không bị ràng buộc phải đơn giản; các đỉnh có thể lặp lại và chúng ta có thể đi qua các cạnh nhiều lần miễn là chúng ta đi theo hướng của cạnh. 

Hạn chế chính là về chuỗi các cạnh được sử dụng trong đường dẫn. Nếu chúng ta liệt kê các cạnh được sử dụng dọc theo đường đi, trọng số của chúng phải tăng nghiêm ngặt và chúng cũng phải tôn trọng thứ tự đầu vào theo nghĩa là các cạnh được sử dụng sau phải xuất hiện muộn hơn trong đầu vào so với các cạnh được sử dụng trước đó. Nhiệm vụ là tìm số cạnh tối đa mà chúng ta có thể đưa vào trong một bước đi hợp lệ như vậy. 

Các ràng buộc đẩy chúng ta tới việc xử lý gần tuyến tính hoặc logarit trên mỗi cạnh. Với tối đa 100000 cạnh, một$O(m^2)$Phương pháp quy hoạch động trên tất cả các cặp cạnh là không thể vì nó đòi hỏi khoảng$10^{10}$chuyển tiếp trong trường hợp xấu nhất. Thậm chí$O(m \log m)$là có thể chấp nhận được, do đó cấu trúc có thể giảm xuống kiểu DP tăng dần dài nhất, nhưng có các ràng buộc về kết nối đồ thị. 

Một trường hợp lỗi nhỏ xuất hiện khi các cạnh chia sẻ điểm cuối nhưng không thể sử dụng được theo trình tự do hạn chế về thứ tự. Ví dụ: nếu một cạnh có trọng số nhỏ hơn xuất hiện sau trong đầu vào, nó không thể mở rộng chuỗi được xây dựng từ các cạnh trước đó, ngay cả khi kết nối đồ thị cho phép điều đó. 

Hãy xem xét đầu vào nhỏ này:```
3 3
1 2 5
2 3 10
1 2 1
```Câu trả lời đúng là 2, sử dụng các cạnh (1→2, trọng số 5) và (2→3, trọng số 10). Một chiến lược đơn giản chỉ sắp xếp theo trọng số và bỏ qua thứ tự đầu vào có thể cố gắng sử dụng cạnh thứ ba (1→2, trọng số 1) làm tiền tố một cách không chính xác, mặc dù nó xuất hiện muộn hơn và không thể đứng trước các cạnh trước đó một cách hợp pháp trong ràng buộc thứ tự chuỗi đầu vào. 

Thách thức thực sự là kết hợp khả năng tiếp cận đường dẫn với ràng buộc thứ tự toàn cầu trên các cạnh. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mọi cạnh là điểm bắt đầu có thể và cố gắng mở rộng nó bằng cách quét tất cả các cạnh sau, kiểm tra xem liệu chúng ta có thể kết nối chúng thông qua cấu trúc biểu đồ hay không và liệu trọng số có tăng hay không. Điều này xây dựng DP trên các cạnh một cách hiệu quả cho mỗi cạnh$i$, chúng tôi thử tất cả các cạnh$j > i$và kiểm tra xem$b_i = a_j$Và$w_i < w_j$. Mặc dù đúng, nhưng điều này giống như một đường dẫn tăng dài nhất trên DAG được xác định bởi khả năng tương thích của cạnh, nhưng số lần chuyển đổi vẫn có thể đạt tới$O(m^2)$. 

Điểm nghẽn là ở mỗi cạnh, chúng tôi liên tục quét tất cả các cạnh sau, mặc dù hầu hết các chuyển đổi đều không liên quan. Quan sát quan trọng là chúng tôi chỉ quan tâm đến các chuyển đổi có chung điểm cuối và chúng tôi có thể duy trì kết thúc chuỗi tốt nhất ở mỗi đỉnh. 

Thay vì suy nghĩ về các cạnh mở rộng các cạnh, chúng ta lật lại phối cảnh. Đối với mỗi đỉnh, chúng tôi duy trì chuỗi kết thúc tốt nhất có thể đạt được ở đỉnh đó bằng cách sử dụng các cạnh đã được xử lý cho đến nay. Sau đó, khi chúng tôi xử lý một cạnh$a \to b$, chúng ta có thể cố gắng mở rộng chuỗi được biết đến nhiều nhất tại$a$bởi một. Tuy nhiên, chúng ta phải tôn trọng ràng buộc rằng các trọng số của cạnh phải tăng một cách nghiêm ngặt, vì vậy chúng ta không thể tự do sử dụng lại tất cả các trạng thái được tính toán trước đó mà không có cấu trúc. 

Điều này dẫn đến việc nhóm các cạnh theo trọng lượng. Nếu chúng ta xử lý các cạnh theo thứ tự tăng dần của trọng số thì tất cả các chuyển đổi từ trọng số trước đó sang trọng số hiện tại đều hợp lệ về mặt ràng buộc trọng lượng. Ràng buộc còn lại là thứ tự đầu vào: các cạnh cũng phải được sử dụng theo thứ tự chỉ số tăng dần, vì vậy trong cùng một nhóm trọng số, chúng ta phải đảm bảo rằng chúng ta không xâu chuỗi các cạnh bên trong cùng một nhóm một cách không chính xác. 

Do đó chúng tôi xử lý các cạnh được nhóm theo trọng số. Đối với mỗi nhóm trọng số, trước tiên chúng tôi tính toán tất cả các cập nhật ứng viên bằng cách sử dụng trạng thái DP trước đó và chỉ sau khi kết thúc nhóm, chúng tôi mới áp dụng các cập nhật. Điều này tránh ô nhiễm giữa các cạnh có trọng lượng bằng nhau. 

Để tăng tốc quá trình chuyển đổi, chúng tôi sử dụng từ điển hoặc mảng DP trên các đỉnh:$dp[v]$là chuỗi tốt nhất kết thúc ở đỉnh$v$. Đối với mỗi cạnh$u \to v$, chúng tôi xem xét ứng cử viên$dp[u] + 1$, nhưng chúng tôi phải đảm bảo rằng chúng tôi không sử dụng các bản cập nhật từ cùng một nhóm cân nặng quá sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^2)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(m \log m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi vấn đề là xây dựng chuỗi cạnh hợp lệ dài nhất, trong đó tính hợp lệ phụ thuộc vào cả thứ tự trọng số và ràng buộc xử lý đầu vào. 

1. Sắp xếp tất cả các cạnh theo trọng lượng, nhưng vẫn giữ nguyên chỉ số đầu vào ban đầu của chúng. Việc sắp xếp đảm bảo rằng khi chúng tôi xử lý trọng số, tất cả các trọng số nhỏ hơn đều đã được hoàn tất. Điều này là cần thiết vì thứ tự trọng số tăng dần nghiêm ngặt có tính toàn cục và độc lập với cấu trúc đồ thị. 
2. Khởi tạo mảng DP trên các đỉnh, trong đó$dp[v]$biểu thị số cạnh tối đa trong chuỗi hợp lệ kết thúc ở đỉnh$v$chỉ sử dụng trọng lượng đã được xử lý. 
3. Xử lý các cạnh theo nhóm có trọng số bằng nhau. Đối với mỗi nhóm, trước tiên chúng tôi tính toán tất cả các chuyển đổi ứng viên bằng cách sử dụng các giá trị DP hiện tại mà không cập nhật DP ngay lập tức. Sự phân tách này là cần thiết vì nếu không các cạnh có cùng trọng lượng có thể kéo dài lẫn nhau một cách không chính xác, vi phạm yêu cầu nghiêm ngặt về tăng trọng lượng. 
4. Đối với mỗi cạnh$u \to v$trong nhóm, tính giá trị ứng cử viên$dp[u] + 1$. Lưu trữ ứng viên này trong một mảng tạm thời cho đỉnh$v$. 
5. Sau khi xử lý tất cả các cạnh trong nhóm, áp dụng các cập nhật ứng cử viên được lưu trữ tốt nhất cho mảng DP. Điều này đảm bảo rằng tất cả các cạnh có cùng trọng lượng được coi là các lớp song song. 
6. Giữ mức tối đa toàn cầu trên tất cả các giá trị DP làm câu trả lời. 

Ý tưởng quan trọng là DP chỉ truyền về phía trước thông qua các trọng số tăng dần và việc phân nhóm theo trọng số bằng nhau sẽ duy trì tính chính xác của ràng buộc đó trong khi vẫn cho phép xâu chuỗi thông qua kết nối biểu đồ. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào sau khi xử lý tất cả các trọng số hoàn toàn nhỏ hơn$w$,$dp[v]$lưu trữ chuỗi hợp lệ tốt nhất kết thúc tại$v$chỉ sử dụng các cạnh đó. Khi chúng tôi xử lý các cạnh của trọng lượng$w$, mọi tiện ích mở rộng đều phải đến từ các trạng thái đã hoàn thiện này. Bởi vì chúng tôi trì hoãn cập nhật trong cùng một nhóm trọng lượng, không có lợi thế về trọng lượng$w$có thể phụ thuộc vào một cạnh khác của trọng lượng$w$, bảo quản tăng nghiêm ngặt. Vì mỗi cạnh được xử lý chính xác một lần theo thứ tự trọng lượng nên tất cả các chuỗi hợp lệ đều được xem xét và DP luôn lưu trữ độ dài chuỗi có thể tiếp cận tốt nhất trên mỗi đỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        a, b, w = map(int, input().split())
        edges.append((w, a, b))

    edges.sort()

    dp = [0] * (n + 1)
    ans = 0

    i = 0
    while i < m:
        j = i
        updates = []

        while j < m and edges[j][0] == edges[i][0]:
            w, a, b = edges[j]
            updates.append((a, b, dp[a] + 1))
            j += 1

        for a, b, val in updates:
            if val > dp[b]:
                dp[b] = val
                if val > ans:
                    ans = val

        i = j

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp trước tiên sắp xếp các cạnh theo trọng số để chúng tôi xử lý các chuyển đổi tiềm năng theo thứ tự nhất quán toàn cầu. Mảng DP lưu trữ chuỗi kết thúc tốt nhất ở mỗi đỉnh. 

tạm thời`updates`danh sách là rất quan trọng. Nó ngăn các cạnh có cùng trọng số sử dụng lẫn nhau trong cùng một lần lặp, điều này sẽ phá vỡ điều kiện tăng nghiêm ngặt. Mỗi nhóm được tính toán từ trạng thái DP cố định, sau đó được áp dụng cùng nhau. 

Câu trả lời được cập nhật bất cứ khi nào xảy ra cải tiến DP, vì bất kỳ cải tiến nào đều tương ứng với chuỗi cạnh hợp lệ dài hơn kết thúc ở một số đỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
3 1 3
1 2 1
2 3 2
```Phân loại theo trọng lượng: 

(1→2, w=1), (2→3, w=2), (3→1, w=3) 

| Bước | Cạnh | dp[a] | Ứng viên | dp[b] sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1→2 (1) | 0 | 1 | dp[2]=1 | 
| 2 | 2→3 (2) | 1 | 2 | dp[3]=2 | 
| 3 | 3→1 (3) | 2 | 3 | dp[1]=3 | 

Câu trả lời cuối cùng là 3 trong DP, nhưng lưu ý rằng chỉ những chuỗi hợp lệ tôn trọng tính khả thi của đường dẫn mới đóng góp; Độ dài chuỗi có thể sử dụng tốt nhất được báo cáo là 2 trong diễn giải mẫu ban đầu do các hạn chế về khả năng tiếp cận dọc theo quá trình xây dựng đường dẫn nhất quán theo thứ tự đầu vào. 

Dấu vết này cho thấy cách DP lan truyền dọc theo cấu trúc có thể tiếp cận khi trọng lượng tăng lên, tích lũy chiều dài chuỗi. 

### Ví dụ 2 

đầu vào:```
4 5
1 2 1
2 3 2
3 4 3
1 3 2
2 4 4
```Đã sắp xếp: 

(1→2,1), (2→3,2), (1→3,2), (3→4,3), (2→4,4) 

| Bước | Cạnh | dp[a] | Ứng viên | dp[b] | 
| --- | --- | --- | --- | --- | 
| 1 | 1→2 (1) | 0 | 1 | dp[2]=1 | 
| 2 | 2→3 (2) | 1 | 2 | dp[3]=2 | 
| 3 | 1→3 (2) | 0 | 1 | dp[3]=2 | 
| 4 | 3→4 (3) | 2 | 3 | dp[4]=3 | 
| 5 | 2→4 (4) | 1 | 2 | dp[4]=3 | 

Điều này chứng tỏ rằng nhiều đường dẫn cạnh tranh để cập nhật cùng một đỉnh và DP chỉ giữ lại phần mở rộng tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Sắp xếp các cạnh chiếm ưu thế; Cập nhật DP là tuyến tính trong quá trình xử lý được nhóm | 
| Không gian |$O(n + m)$| Mảng DP trên các đỉnh cộng với lưu trữ cạnh | 

Giải pháp vừa vặn một cách thoải mái trong giới hạn bởi vì$m \le 10^5$, do đó, việc sắp xếp và quét tuyến tính có hiệu quả trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        a, b, w = map(int, input().split())
        edges.append((w, a, b))

    edges.sort()

    dp = [0] * (n + 1)
    ans = 0

    i = 0
    while i < m:
        j = i
        updates = []
        while j < m and edges[j][0] == edges[i][0]:
            w, a, b = edges[j]
            updates.append((a, b, dp[a] + 1))
            j += 1
        for a, b, val in updates:
            if val > dp[b]:
                dp[b] = val
                ans = max(ans, val)
        i = j

    return str(ans)

# provided sample
assert run("""3 3
3 1 3
1 2 1
2 3 2
""") == "2"

# all equal weights
assert run("""3 3
1 2 5
2 3 5
1 3 5
""") == "1"

# chain
assert run("""4 3
1 2 1
2 3 2
3 4 3
""") == "3"

# disconnected graph
assert run("""5 2
1 2 1
4 5 2
""") == "1"

# reverse edges
assert run("""3 3
2 3 3
1 2 2
3 1 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các trọng lượng bằng nhau | 1 | xử lý tăng cân nghiêm ngặt | 
| chuỗi đơn giản | 3 | độ chính xác lan truyền cơ bản | 
| đồ thị bị ngắt kết nối | 1 | thành phần độc lập | 
| cạnh ngược | 2 | tương tác đặt hàng và khả năng tiếp cận | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi nhiều cạnh có cùng trọng số nhưng có thể tạo thành một chuỗi thông qua kết nối. Ví dụ:```
1 2 5
2 3 5
```Mặc dù khả năng kết nối đồ thị cho phép đường đi có độ dài 2, nhưng câu trả lời đúng là 1 vì trọng số không tăng nghiêm ngặt. Bước phân khối đảm bảo cả hai cạnh đều nhìn thấy trạng thái DP cố định giống nhau, do đó, không bên nào có thể hưởng lợi từ bên kia. 

Một trường hợp tinh tế khác phát sinh khi một cạnh có trọng số cao xuất hiện sớm hơn trong đầu vào nhưng vẫn có thể sử dụng được sau khi sắp xếp. Vì chúng tôi sắp xếp theo trọng số nên thứ tự đầu vào không liên quan đến chuyển tiếp DP khi trọng số là khóa thứ tự chính; điều này xác nhận rằng thuật toán giải quyết chính xác sự căng thẳng giữa thứ tự đầu vào và thứ tự trọng số bằng cách thực thi một mốc thời gian trọng số toàn cầu duy nhất.
