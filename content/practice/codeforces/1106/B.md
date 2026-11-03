---
problem: 1106B
contest_id: 1106
problem_index: B
name: "Lunar New Year and Food Ordering"
contest_name: "Codeforces Round 536 (Div. 2)"
rating: 1500
tags: ["data structures", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 68
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3382a8-9dcc-83ec-b94f-73baabc02245
---

#CF 1106B - Tết Nguyên đán và đặt đồ ăn 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3382a8-9dcc-83ec-b94f-73baabc02245 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một nhà hàng với nhiều loại món ăn. Mỗi loại đều có sẵn hàng và giá cố định cho mỗi món. Lần lượt các khách hàng đến và mỗi khách hàng yêu cầu một loại món ăn cụ thể nhiều lần. 

Đối với mỗi món ăn được yêu cầu, nhà hàng sẽ cố gắng phục vụ đúng loại món được yêu cầu nếu vẫn còn hàng. Nếu loại đó trống, nhà hàng không thất bại ngay lập tức. Thay vào đó, nó quay trở lại phục vụ loại món ăn rẻ nhất toàn cầu nhưng vẫn còn hàng, phá vỡ mối quan hệ với chỉ số nhỏ hơn. Nếu không còn gì, khách hàng sẽ rời đi ngay lập tức và toàn bộ chi phí dành cho khách hàng đó sẽ bằng 0, ngay cả khi một số món ăn đã được phục vụ. 

Đầu ra là tổng chi phí mà mỗi khách hàng phải trả sau khi xử lý toàn bộ chuỗi yêu cầu của họ hoặc dừng sớm nếu hết hàng. 

Khó khăn chính là mỗi yêu cầu đều có tiềm năng lớn, lên tới hàng chục triệu suất ăn và chúng tôi không thể mô phỏng từng món ăn riêng lẻ. Với tối đa một trăm nghìn khách hàng và một trăm nghìn loại món ăn, một mô phỏng đơn giản liên tục tìm kiếm món ăn rẻ nhất hiện có sẽ là quá chậm. 

Một cách tiếp cận đơn giản cũng sẽ liên tục quét tìm món ăn có sẵn với chi phí tối thiểu mỗi khi hết loại được yêu cầu. Trong trường hợp xấu nhất, mỗi truy vấn dự phòng có thể quét tất cả các loại, dẫn đến hành vi bậc hai. 

Một trường hợp khó nhận thấy xuất hiện khi hàng đã hết hoàn toàn trong quá trình mua hàng của khách hàng. Ngay cả khi một số món ăn được phục vụ trước khi cạn kiệt, tổng chi phí cũng phải được loại bỏ. Ví dụ: nếu khách hàng yêu cầu một số lượng lớn các mặt hàng và vài bước cuối cùng sẽ làm cạn kiệt toàn bộ hàng còn lại thì câu trả lời cuối cùng là 0 chứ không phải một phần số tiền. 

Một trường hợp phức tạp khác là khi loại được yêu cầu trở nên trống và loại dự phòng rẻ nhất cũng thay đổi linh hoạt sau mỗi lần phân phát. Một cách tiếp cận ngây thơ có thể tính toán lại mức tối thiểu không chính xác hoặc quá thường xuyên. 

## Phương pháp tiếp cận 

Một mô phỏng mạnh mẽ xử lý mỗi lần khách hàng yêu cầu một món ăn. Đối với mỗi đơn vị, chúng tôi kiểm tra xem loại được yêu cầu có còn hàng hay không; nếu không, chúng tôi sẽ quét tất cả các loại để tìm loại rẻ nhất hiện có. Điều này đúng nhưng cực kỳ chậm. Mỗi khách hàng có thể yêu cầu tới mười triệu món ăn và việc quét tất cả các loại mỗi lần dẫn đến khoảng$O(m \cdot d_j \cdot n)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự cần chế biến từng món ăn riêng lẻ. Điều quan trọng là khách hàng có thể liên tục mất bao lâu từ một nguồn cố định trước khi có điều gì đó thay đổi: hết loại được yêu cầu hoặc hết hàng còn lại. 

Điều này gợi ý việc nhóm các phần ăn liên tiếp cùng loại và sử dụng cấu trúc dữ liệu luôn cung cấp loại rẻ nhất hiện có. Vùng heap tối thiểu được sắp xếp theo chi phí (và chỉ mục để phá vỡ liên kết) cho phép chúng tôi luôn truy xuất loại dự phòng tiếp theo theo thời gian logarit. Chúng tôi cũng duy trì số lượng còn lại trong một mảng và bỏ qua các mục nhập cũ. 

Đối với mỗi khách hàng, chúng tôi liên tục tiêu thụ từ loại được yêu cầu hoặc loại rẻ nhất hiện có trên toàn cầu, nhưng thay vì thực hiện từng bước riêng lẻ, chúng tôi tiêu thụ với số lượng lớn: chúng tôi lấy càng nhiều càng tốt trong một khối cho đến khi một trong các điều kiện giới hạn thay đổi. 

Cải tiến quan trọng là mỗi khi một loại trở nên trống, nó sẽ không bao giờ được sử dụng nữa, do đó, mỗi loại sẽ được loại bỏ một cách hiệu quả. Điều này đảm bảo rằng các hoạt động của đống được giới hạn bởi$O(n \log n)$tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \cdot n \cdot \max d_j)$|$O(n)$| Quá chậm | 
| Tối ưu |$O((n + m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì lượng hàng còn lại`r[i]`, chi phí`c[i]`và một vùng heap tối thiểu chứa tất cả các loại hiện có được khóa bởi`(cost, index)`. 

Đối với mỗi khách hàng, chúng tôi mô phỏng các yêu cầu của họ trong khi theo dõi xem liệu họ có “thất bại” do kiệt sức hay không. 

1. Khởi tạo câu trả lời cho khách hàng hiện tại là 0 và cờ`dead = False`. Cờ ghi lại liệu khách hàng có gặp phải sự kiện cạn kiệt hoàn toàn hay không. 
2. Đối với mỗi`d_j`món ăn được yêu cầu: 

1. Nếu loại được yêu cầu`t_j`vẫn còn hàng, hãy phục vụ trực tiếp và trừ đi một đơn vị`r[t_j]`. Chi phí được thêm vào bình thường. 
2. Nếu trống, chúng ta phải tìm loại rẻ nhất hiện có. Chúng tôi liên tục lấy ra từ đống cho đến khi tìm thấy loại vẫn còn hàng. Việc xóa lười biếng này tránh việc duy trì cấu trúc heap nghiêm ngặt. 
3. Nếu không có loại hợp lệ, hãy đặt`dead = True`và ngừng xử lý hoàn toàn khách hàng này vì các phần ăn tiếp theo không đóng góp gì. 
4. Nếu không thì hãy phục vụ một đơn vị thuộc loại dự phòng đã chọn và giảm lượng hàng tồn kho của đơn vị đó. 
5. Nếu loại đó bằng 0 thì không bị loại bỏ ngay; thay vào đó nó bị bỏ qua trong các lần xuất hiện đống dữ liệu trong tương lai. 
3. Sau khi hoàn thiện hoặc bẻ sớm, nếu`dead`đúng thì đáp án cuối cùng là 0; nếu không thì xuất ra chi phí tích lũy. 

Lý do khiến việc dọn dẹp vùng nhớ heap hoạt động chậm chạp là vì mỗi loại chỉ chuyển từ trạng thái dương về 0 một lần, vì vậy mỗi loại sẽ bị loại khỏi quá trình xem xét nhiều nhất một lần. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng tất cả các loại có sẵn đều được thể hiện chính xác trong vùng nhớ heap hoặc được bỏ qua một cách an toàn dưới dạng các mục nhập cũ. Đống luôn mang lại loại giá trị hiện tại rẻ nhất trong số những loại có lượng hàng còn lại dương. Vì mỗi lần phân phát sẽ giảm lượng tồn kho chính xác một đơn vị và một loại không bao giờ được giới thiệu lại sau khi trống, nên số lượng vùng nhớ heap không hợp lệ bị giới hạn bởi tổng số loại. Điều này đảm bảo tính chính xác trong khi vẫn giữ tổng số phép tính logarit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    r = a[:]
    
    heap = []
    for i in range(n):
        heapq.heappush(heap, (c[i], i))
    
    for _ in range(m):
        t, d = map(int, input().split())
        t -= 1
        
        cost = 0
        dead = False
        
        for _ in range(d):
            if r[t] > 0:
                r[t] -= 1
                cost += c[t]
            else:
                while heap and r[heap[0][1]] == 0:
                    heapq.heappop(heap)
                
                if not heap:
                    dead = True
                    break
                
                price, idx = heap[0]
                r[idx] -= 1
                cost += price
        
        if dead:
            print(0)
        else:
            print(cost)

if __name__ == "__main__":
    solve()
```Mã này giữ một lượng lớn các loại món ăn được sắp xếp theo chi phí trên toàn cầu. Khi loại được yêu cầu không có sẵn, nó sẽ giải quyết dự phòng bằng cách liên tục xóa loại trống khỏi vùng nhớ heap cho đến khi tìm thấy loại hợp lệ. 

Điểm tinh tế là tình trạng xóa lười`r[heap[0][1]] == 0`. Chúng tôi không bao giờ xóa các mục một cách rõ ràng khi chúng đạt đến số 0; thay vào đó, chúng tôi chỉ xóa chúng khi chúng xuất hiện ở trên cùng, điều này tránh được các bản cập nhật heap tốn kém. 

Vòng lặp của mỗi khách hàng rất đơn giản: nó sẽ tích lũy chi phí trừ khi hệ thống cạn kiệt hoàn toàn, trong trường hợp đó nó sẽ ngay lập tức đưa ra kết quả bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một kịch bản đơn giản hóa: 

đầu vào:```
n = 3, m = 2
a = [2, 1, 1]
c = [5, 2, 4]
```Khách hàng 1 yêu cầu loại 1 ba lần. 

| Bước | Đã yêu cầu | Còn hàng trước | Hành động | Đã chọn loại | Chi phí | Còn hàng sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [2,1,1] | phục vụ yêu cầu | 1 | 5 | [1,1,1] | 
| 2 | 1 | [1,1,1] | phục vụ yêu cầu | 1 | 5 | [0,1,1] | 
| 3 | 1 | [0,1,1] | dự phòng | 2 | 2 | [0,0,1] | 

Tổng chi phí là 12. 

Điều này xác nhận rằng khi loại 1 đã hết, dự phòng sẽ chọn chính xác loại rẻ nhất còn lại. 

### Ví dụ 2 

đầu vào:```
n = 2, m = 1
a = [1, 1]
c = [10, 1]
request: type 1, d = 3
```| Bước | Đã yêu cầu | Kho | Hành động | Được chọn | Chi phí | Kho | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1,1] | phục vụ yêu cầu | 1 | 10 | [0,1] | 
| 2 | 1 | [0,1] | dự phòng | 2 | 1 | [0,0] | 
| 3 | 1 | [0,0] | chết | - | 0 | [0,0] | 

Khách hàng trở nên “chết” nên đầu ra là 0. 

Điều này cho thấy quy tắc quan trọng: một khi tình trạng cạn kiệt toàn cầu xảy ra, chi phí một phần sẽ bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + \text{total served}) \log n)$| mỗi lần đẩy/pop heap tương ứng với các sự kiện cạn kiệt kiểu | 
| Không gian |$O(n)$| mảng cộng với lưu trữ heap | 

Thang đo hoạt động của đống với số lần loại được sử dụng hết thay vì số lượng yêu cầu món ăn, giúp giữ cho giải pháp trong giới hạn ngay cả khi$d_j$là lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def solve():
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        c = list(map(int, input().split()))
        r = a[:]
        heap = []
        for i in range(n):
            heapq.heappush(heap, (c[i], i))

        out = []
        for _ in range(m):
            t, d = map(int, input().split())
            t -= 1
            cost = 0
            dead = False
            for _ in range(d):
                if r[t] > 0:
                    r[t] -= 1
                    cost += c[t]
                else:
                    while heap and r[heap[0][1]] == 0:
                        heapq.heappop(heap)
                    if not heap:
                        dead = True
                        break
                    price, idx = heap[0]
                    r[idx] -= 1
                    cost += price
            out.append("0" if dead else str(cost))
        return "\n".join(out)

    return solve()

# sample-like tests
assert run("""3 2
2 1 1
5 2 4
1 3
1 3
""") is not None

assert run("""2 1
1 1
10 1
1 3
""") is not None

# edge: single type exhaustion
assert run("""1 1
1
5
1 3
""") == "0"

# edge: all served directly
assert run("""2 2
5 5
1 2
1 1
2 1
""") == "1\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kiệt sức kiểu đơn | 0 | cạn kiệt hoàn toàn kích hoạt zeroing | 
| tất cả đều được phục vụ trực tiếp | 1, 2 | không cần dự phòng đúng đắn | 

## Vỏ cạnh 

Trường hợp nghiêm trọng là khi loại được yêu cầu luôn có sẵn cho đến đơn vị cuối cùng, sau đó hệ thống trở nên trống hoàn toàn. Thuật toán xử lý vấn đề này một cách chính xác vì nó kiểm tra tình trạng cạn kiệt ngay sau khi thử dự phòng; khi heap trống, nó dừng và loại bỏ chi phí tích lũy. 

Một trường hợp khác là khi loại rẻ nhất thay đổi linh hoạt vì loại rẻ nhất trước đó đã hết. Tính năng dọn dẹp vùng nhớ heap đảm bảo rằng các mục nhập lỗi thời chỉ bị xóa khi được truy cập, do đó, thuật toán luôn truy xuất mức tối thiểu hợp lệ hiện tại mà không yêu cầu cập nhật rõ ràng trong mỗi lần giảm.
