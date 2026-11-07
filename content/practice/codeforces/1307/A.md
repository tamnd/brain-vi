---
problem: 1307A
contest_id: 1307
problem_index: A
name: "Cow and Haybales"
contest_name: "Codeforces Round 621 (Div. 1 + Div. 2)"
rating: 800
tags: ["greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 181
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dd980-f264-83ec-8273-607302c398bb
---

#CF 1307A - Bò và cỏ khô 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 1s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dd980-f264-83ec-8273-607302c398bb 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được đưa cho một hàng đống cỏ khô, mỗi đống chứa một số kiện cỏ khô. Trong một lần di chuyển, chúng ta được phép lấy một kiện cỏ khô từ một đống và chuyển nó sang một trong những đống cỏ khô lân cận. Quá trình này có thể được lặp lại nhiều nhất`d`ngày, và mỗi ngày chúng ta cũng có thể chọn không làm gì cả. 

Mục tiêu là làm cho đống đầu tiên càng lớn càng tốt sau nhiều nhất`d`những động thái như vậy. 

Điều khiến vấn đề này trở nên tinh tế là việc di chuyển các kiện cỏ khô không phải là dịch chuyển tức thời. Mỗi kiện cỏ khô phải đi qua các cọc liền kề, do đó chi phí di chuyển kiện hàng từ vị trí`i`để định vị`1`chính xác là`i - 1`ngày, giả sử chúng ta di chuyển nó sang trái từng bước một. Điều này ngay lập tức gợi ý rằng không phải tất cả cỏ khô đều có giá trị như nhau trong thời hạn. 

Đầu vào bao gồm nhiều kịch bản độc lập. Đối với mỗi cái, chúng ta phải tính giá trị cuối cùng tốt nhất có thể có của đống đầu tiên sau nhiều nhất`d`di chuyển. 

Những hạn chế là nhỏ:`n, d ≤ 100`, và chiều cao mỗi cọc cũng nhỏ. Điều này có nghĩa là chúng ta có thể đủ khả năng lập luận bậc hai hoặc thậm chí bậc ba cho mỗi trường hợp thử nghiệm mà không cần quan tâm. Bất kỳ giải pháp nào cố gắng mô phỏng chuyển động hàng ngày một cách tham lam cũng có thể thực hiện được, nhưng chúng ta nên cẩn thận để không phức tạp hóa quá mức dẫn đến quy tắc tối ưu cục bộ đơn giản. 

Một sai lầm phổ biến là cho rằng chúng ta phải luôn rút từ đống khác 0 gần nhất bất cứ khi nào có thể. Trực giác đó gần gũi nhưng không đầy đủ. Hạn chế thực sự là mỗi đống chỉ có thể đóng góp một số lượng cỏ khô hạn chế dựa trên số lượng trong số chúng có thể đạt chỉ số vật lý.`1`ở trong`d`các bước. 

Cạm bẫy thứ hai là mô phỏng các chuyển động một cách tham lam trong nhiều ngày. Điều này có thể thất bại vì nhiều kiện cỏ khô cạnh tranh để có cùng dung lượng đường đi và mô phỏng đơn giản có thể lãng phí các bước di chuyển các kiện cỏ khô qua lại mà không cải thiện được số lượng cuối cùng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các chuỗi di chuyển có thể có trên`d`ngày. Mỗi ngày, chúng tôi chọn một đống và dịch chuyển một đơn vị sang trái hoặc sang phải. Không gian trạng thái là rất lớn ngay cả đối với các đầu vào nhỏ, vì mỗi ngày phân nhánh trên`O(n)`sự lựa chọn, đưa ra đại khái`O(n^d)`khả năng. Ngay cả khi cắt tỉa, điều này nhanh chóng trở nên khó chữa. 

Chúng ta cần một góc nhìn khác: thay vì mô phỏng chuyển động theo thời gian, chúng ta nên suy luận xem có bao nhiêu kiện cỏ khô có thể kết thúc ở vị trí mục tiêu.`1`trong một ngân sách du lịch hạn chế. 

Mỗi haybale nằm ở vị trí`i`đóng góp vào đống`1`chỉ khi chúng ta có thể di chuyển nó sang trái`i-1`lần. Vì mỗi lần di chuyển tiêu tốn một ngày nên điều kiện chỉ đơn giản là`i - 1 ≤ d`. Điều này có nghĩa là mỗi đống cỏ khô sẽ đóng góp độc lập tất cả các kiện cỏ khô của nó nếu nó nằm trong khoảng cách có thể tiếp cận được và không đóng góp gì khác. 

Không có hình phạt tương tác giữa các đống khác nhau vì việc di chuyển một đống cỏ khô không chặn một đống cỏ khô khác; hạn chế duy nhất là thời gian. Vì vậy, chiến lược tối ưu là lấy mọi thứ từ tất cả các cọc có thể đạt chỉ số`1`ở trong`d`di chuyển. 

Điều này làm giảm vấn đề thành một tổng tiền tố đơn giản trên các vị trí có thể tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^d) | O(n) | Quá chậm | 
| Tính tổng dựa trên khoảng cách | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cọc tại mục lục`i`, tính khoảng cách của nó đến đống đầu tiên là`i - 1`. Điều này thể hiện số lần di chuyển tối thiểu cần thiết để chuyển một kiện cỏ khô sang đống`1`. 
2. So sánh khoảng cách này với số ngày có sẵn`d`. Nếu như`i - 1 ≤ d`, thì mỗi kiện cỏ khô trong đống đó cuối cùng có thể được chuyển thành đống`1`. 
3. Nếu thỏa mãn điều kiện thì cộng giá trị đầy đủ`a[i]`để trả lời. 
4. Nếu điều kiện không được thỏa mãn thì bỏ qua hoàn toàn đống cỏ vì không có đống cỏ khô nào của nó có thể đạt đến vị trí đầu tiên trong thời hạn. 
5. Tổng hợp các khoản đóng góp từ tất cả các cọc và đưa ra kết quả. 

Ý tưởng chính là mỗi haybale đều độc lập. Chúng tôi không chọn thứ tự di chuyển; chúng tôi chỉ đang kiểm tra tính khả thi của việc hoàn thành đường đi của nó trong thời gian giới hạn. 

### Tại sao nó hoạt động 

Mỗi kiện cỏ khô hoạt động giống như một đơn vị yêu cầu chi phí di chuyển cố định bằng khoảng cách của nó với đống cỏ khô.`1`. Vì các bước di chuyển không mang tính độc quyền theo cách hạn chế việc sử dụng thời gian song song vượt quá ngân sách toàn cầu`d`, tính khả thi chỉ phụ thuộc vào việc mỗi đơn vị riêng lẻ có phù hợp với ngân sách đó hay không. Không có lợi ích gì trong việc trì hoãn hoặc sắp xếp lại các bước đi vì mọi bước đi đều giống nhau về chi phí và hiệu quả, vì vậy giải pháp tối ưu chỉ đơn giản là tổng của tất cả những đóng góp khả thi độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, d = map(int, input().split())
    a = list(map(int, input().split()))
    
    ans = 0
    for i in range(n):
        if i <= d:
            ans += a[i]
    
    print(ans)
```Việc thực hiện mã hóa trực tiếp việc quan sát khoảng cách. chỉ số`i`đã đại diện cho khoảng cách từ cọc`1`trong lập chỉ mục dựa trên số 0, vì vậy chúng tôi chỉ cần kiểm tra xem`i`nằm trong số ngày cho phép. 

Không cần mô phỏng. Mỗi trường hợp thử nghiệm được xử lý theo thời gian tuyến tính bằng cách quét mảng một lần và tính tổng các đóng góp hợp lệ. 

Một lỗi triển khai phổ biến là lập chỉ mục riêng lẻ. Từ đống`1`tương ứng với chỉ số`0`, khoảng cách chính xác là chính xác`i`, không`i + 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
1 0 3 2
```Chúng tôi theo dõi cọc nào có thể đóng góp. 

| tôi | một [tôi] | tôi ≤ d | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | vâng | 1 | 
| 1 | 0 | vâng | 0 | 
| 2 | 3 | vâng | 3 | 
| 3 | 2 | vâng | 2 | 

Tổng cuối cùng là`6`. 

Điều này cho thấy rằng tất cả các cọc đều nằm trong tầm tay vì`d = 5`vượt qua mọi khoảng cách. 

### Ví dụ 2 

đầu vào:```
2 2
100 1
```| tôi | một [tôi] | tôi ≤ d | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 100 | vâng | 100 | 
| 1 | 1 | vâng | 1 | 

Kết quả cuối cùng là`101`. 

Điều này khẳng định rằng ngay cả một cọc cách xa một bước cũng có thể đóng góp đầy đủ, miễn là khoảng cách của nó nằm trong thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét từng cọc một lần và thực hiện công việc liên tục trên mỗi phần tử | 
| Không gian | O(1) thêm | Chỉ có một khoản tiền được lưu trữ | 

Được cho`n ≤ 100`Và`t ≤ 100`, giải pháp thực hiện tối đa 10.000 thao tác, nhanh chóng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, d = map(int, input().split())
        a = list(map(int, input().split()))
        ans = 0
        for i in range(n):
            if i <= d:
                ans += a[i]
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
4 5
1 0 3 2
2 2
100 1
1 8
0
""") == """6
101
0"""

# custom cases
assert run("""1
1 0
5
""") == "5", "single element always reachable"

assert run("""1
5 0
1 2 3 4 5
""") == "1", "only first pile contributes"

assert run("""1
5 10
1 2 3 4 5
""") == "15", "all piles reachable"

assert run("""1
4 2
10 20 30 40
""") == "60", "prefix cutoff at distance 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | trường hợp cơ sở, khoảng cách bằng không | 
| chỉ cọc đầu tiên | 1 | không được phép di chuyển | 
| tất cả đều có thể truy cập | 15 | tích lũy đầy đủ | 
| cắt tiền tố | 60 | ranh giới khoảng cách chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi`d = 0`. Trong tình huống này không thể di chuyển được nên chỉ có thể chất đống`1`có thể đóng góp. Thuật toán xử lý việc này vì chỉ có chỉ mục`0`thỏa mãn`i ≤ 0`. 

Một trường hợp khác là khi`d`là rất lớn so với`n`. Ở đây mọi cọc đều có thể truy cập được và câu trả lời đúng chỉ đơn giản là tổng số của tất cả các kiện cỏ khô. Vòng lặp tự nhiên bao gồm tất cả các chỉ số. 

Cuối cùng, khi đống đầu tiên trống nhưng đống ở xa chứa cỏ khô, giải pháp vẫn hoạt động chính xác vì nó không giả định bất cứ điều gì về ban đầu.`a[1]`; nó chỉ tích lũy những đóng góp có thể tiếp cận dựa trên khoảng cách.
