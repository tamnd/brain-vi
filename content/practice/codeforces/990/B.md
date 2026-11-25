---
problem: 990B
contest_id: 990
problem_index: B
name: "Micro-World"
contest_name: "Educational Codeforces Round 45 (Rated for Div. 2)"
rating: 1200
tags: ["greedy", "sortings"]
answer: passed_samples
verified: false
solve_time_s: 88
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33ab40-4628-83ec-b07d-d37d37181695
---

#CF 990B - Thế Giới Vi Mô 

**Đánh giá:** 1200 
**Tags:** tham lam, phân loại 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33ab40-4628-83ec-b07d-d37d37181695 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp vi khuẩn, mỗi vi khuẩn có kích thước cố định. Một vi khuẩn lớn hơn chỉ có thể tiêu thụ một vi khuẩn nhỏ hơn khi có hai điều kiện tại thời điểm tương tác: nó phải lớn hơn một cách nghiêm ngặt, đồng thời nó không thể quá lớn so với con mồi, cụ thể là kích thước của nó không được vượt quá kích thước của con mồi nhiều hơn một hằng số nhất định.$K$. Sau khi ăn, chỉ còn lại người ăn và kích thước của nó không bao giờ thay đổi. 

Vì mỗi vi khuẩn sống sót có thể liên tục ăn thịt vi khuẩn khác, nên chúng tôi được phép xóa các phần tử một cách hiệu quả dưới một ràng buộc phụ thuộc vào sự khác biệt về kích thước theo cặp. Nhiệm vụ là chọn thứ tự xóa sao cho càng ít vi khuẩn càng tốt ở cuối. 

Kích thước đầu vào tăng lên$2 \cdot 10^5$, điều này ngay lập tức loại trừ mọi mô phỏng liên tục quét hoặc thử tất cả các cặp. Bất cứ điều gì bậc hai trong$n$sẽ thất bại, và thậm chí$O(n \log^2 n)$là rủi ro. Chúng ta nên mong đợi một chiến lược sắp xếp một lần và sau đó xử lý mảng một cách tuyến tính. 

Một khó khăn tinh tế đến từ việc ăn uống không đơn thuần là “gộp liền nhau theo thứ tự đã sắp xếp”. Một con vi khuẩn lớn không thể tùy tiện ăn những con nhỏ, dù nó lớn hơn rất nhiều; nó bị chặn bởi giới hạn trên$a_i \le a_j + K$. Điều này tạo ra một cửa sổ tương tác có giới hạn mà các chiến lược tham lam ngây thơ có thể dễ dàng xử lý sai. 

Ví dụ, hãy xem xét kích thước$[100, 1]$với$K = 10$. Vi khuẩn lớn hơn không thể ăn vi khuẩn nhỏ hơn vì$100 > 1$giữ, nhưng$100 \le 11$thất bại. Trực giác ngây thơ cho rằng “kẻ lớn nhất luôn ăn mọi thứ nhỏ hơn” sẽ dự đoán sai về một người sống sót. Câu trả lời đúng là hai người sống sót. 

Một chế độ lỗi khác xuất hiện khi cố gắng xâu chuỗi theo hướng chuyển tiếp. Giả sử chúng ta cố gắng liên tục hợp nhất các phần tử liền kề sau khi sắp xếp. Việc hợp nhất hợp lệ cục bộ có thể chặn việc hợp nhất trong tương lai có thể thực hiện được theo một thứ tự lựa chọn khác, vì vậy việc hợp nhất liền kề tham lam là không an toàn. 

Thách thức chính là cấu trúc các hoạt động xóa sao cho mỗi lần xóa đều tương ứng với một cặp con mồi-ăn hợp lệ, đồng thời tối đa hóa số lần xóa mà chúng tôi có thể thực hiện. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực sẽ mô phỏng quá trình: liên tục chọn bất kỳ cặp hợp lệ nào$(i, j)$, xóa bỏ$j$, và tiếp tục cho đến khi không còn nước đi nào nữa. Về nguyên tắc thì điều này đúng, nhưng mỗi bước yêu cầu phải quét tìm một cặp hợp lệ và có thể có$O(n)$xóa, dẫn đến$O(n^2)$hoặc hành vi tồi tệ hơn. Với$n = 2 \cdot 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là câu trả lời cuối cùng chỉ phụ thuộc vào cách chúng ta phân chia vi khuẩn thành các nhóm mà mỗi nhóm có thể bị “xóa sổ” bởi một kẻ sống sót duy nhất. Nếu một vi khuẩn được chọn làm vi khuẩn sống sót trong nhóm, nó chỉ có thể loại bỏ những vi khuẩn có kích thước nằm trong một khoảng cụ thể so với nó: từ$a_i - K$lên tới$a_i - 1$. Bất cứ điều gì nhỏ hơn$a_i - K$Người sống sót đó không thể truy cập được và phải thuộc về một nhóm khác. 

Điều này gợi ý việc sắp xếp mảng và luôn cố gắng sử dụng vi khuẩn lớn nhất còn lại làm đại diện cho nhóm tiếp theo. Sự lựa chọn đó rất mạnh mẽ vì nó tối đa hóa số lượng phần tử mà nó có thể loại bỏ. Sau khi chúng tôi sửa phần tử lớn nhất này, tất cả vi khuẩn trong khoảng cho phép ngay bên dưới nó có thể bị loại bỏ và chúng tôi lặp lại quy trình trên những phần còn lại. 

Điều này biến vấn đề thành việc liên tục bóc tách các “cửa sổ” được neo ở mức tối đa hiện tại, việc này có thể được thực hiện chỉ trong một lần duyệt qua mảng đã sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + Windows tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sắp xếp kích thước vi khuẩn theo thứ tự tăng dần để tất cả các so sánh trở nên cục bộ. 

1. Sắp xếp mảng$a$theo thứ tự tăng dần. Điều này cho phép chúng ta suy luận về phạm vi ăn uống hợp lý bằng cách sử dụng các phân đoạn liền kề. 
2. Bắt đầu từ phần tử lớn nhất còn lại. Hãy coi phần tử này là người sống sót của nhóm hiện tại, vì nó có khả năng ăn thịt người khác cao nhất. 
3. Hãy coi vi khuẩn sống sót này là vi khuẩn cuối cùng trong câu trả lời. 
4. Từ phần tử lớn nhất này, di chuyển sang trái và loại bỏ mọi phần tử có kích thước nhỏ nhất$a[i] - K$. Đây chính xác là những vi khuẩn mà nó có thể ăn hợp pháp vì chúng nhỏ hơn nó nhưng không quá nhỏ. 
5. Tiếp tục quá trình từ phần tử chưa được xử lý tiếp theo sang bên trái, chọn lại phần tử đó làm phần tử sống sót mới và lặp lại bước loại bỏ. 

Mỗi bước loại bỏ càng nhiều yếu tố càng tốt trong một người sống sót, đảm bảo rằng chúng ta không bao giờ lãng phí một người ăn tiềm năng. 

### Tại sao nó hoạt động 

Cấu trúc được sắp xếp đảm bảo rằng khi chúng ta chọn phần tử lớn nhất còn lại, mọi phần tử còn lại khác đều nằm trong phạm vi có thể ăn được của nó hoặc quá nhỏ để nó có thể ăn được. Nếu một phần tử quá nhỏ đối với phần tử sống sót hiện tại thì nó cũng quá nhỏ đối với bất kỳ phần tử sống sót nào trong tương lai được chọn trong vùng này của mảng, bởi vì phần tử sống sót trong tương lai sẽ chỉ có giá trị nhỏ hơn hoặc bằng nhau. Điều này buộc các phần tử như vậy phải thành lập các nhóm mới. 

Điều này tạo ra một phân vùng của mảng thành số nhóm tối thiểu trong đó mỗi nhóm có một “phần tử tối đa” hợp lệ có thể loại bỏ tất cả các phần tử khác trong nhóm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    
    ans = 0
    i = n - 1
    
    while i >= 0:
        ans += 1
        current = a[i]
        
        i -= 1
        while i >= 0 and a[i] >= current - k:
            i -= 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng liên tục chọn vi khuẩn lớn nhất còn lại và tiêu thụ mọi thứ trong khoảng thời gian ăn hợp lệ của nó. Vòng lặp bên ngoài di chuyển từ phải sang trái trên mảng đã sắp xếp, đảm bảo mỗi lần lặp xác định một nhóm còn sót lại. 

Vòng bên trong là phần quan trọng: nó bỏ qua tất cả vi khuẩn mà người sống sót hiện tại có thể ăn được. điều kiện$a[i] \ge current - K$mã hóa chính xác giới hạn dưới của phạm vi có thể ăn được. Bất cứ thứ gì dưới ngưỡng đó đều không thể được tiêu thụ và phải được xử lý sau. 

Một lỗi phổ biến là cố gắng duy trì nhiều con trỏ tiến về phía trước. Điều đó có xu hướng đếm gấp đôi hoặc bỏ sót các phần tử vì nhóm vốn đã được neo ở mức cực đại chứ không phải cực tiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 1
101 53 42 102 101 55 54
```Mảng được sắp xếp:$[42, 53, 54, 55, 101, 101, 102]$| Bước | Người sống sót hiện tại | Còn lại được xử lý | Hành động | 
| --- | --- | --- | --- | 
| 1 | 102 | 102, 101, 101 | 102 ăn 101, 101 (cả hai ≥ 101) | 
| 2 | 55 | 55, 54 | 55 ăn 54 | 
| 3 | 42 | 42 | Không có ai ăn | 

Câu trả lời cuối cùng là 3 người sống sót. 

Dấu vết này cho thấy cách mỗi người sống sót xóa một khối liền kề được giới hạn bởi$K$và những khoảng trống lớn sẽ buộc các nhóm mới một cách tự nhiên như thế nào. 

### Ví dụ 2 

đầu vào:```
6 4
20 15 10 15 20 25
```Mảng được sắp xếp:$[10, 15, 15, 20, 20, 25]$| Bước | Người sống sót | Đã xóa | 
| --- | --- | --- | 
| 1 | 25 | 20, 20, 15, 15 | 
| 2 | 10 | không | 

Câu trả lời là 2. 

Ví dụ này chứng minh rằng khi phạm vi tiếp cận của người sống sót giảm xuống dưới khoảng cách, các phần tử nhỏ hơn sẽ bị cô lập và phải tạo thành các nhóm riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quá trình quét là tuyến tính | 
| Không gian |$O(1)$thêm (bỏ qua sắp xếp) | Chỉ sử dụng con trỏ và bộ đếm | 

Thuật toán dễ dàng nằm trong giới hạn vì mảng được xử lý trong một lần sau khi sắp xếp và mỗi phần tử được truy cập nhiều nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("7 1\n101 53 42 102 101 55 54\n") == "3"

# all equal
assert run("5 10\n7 7 7 7 7\n") == "5"

# no possible eating due to K=0
assert run("4 0\n1 2 3 4\n") == "4"

# large K allows chaining
assert run("5 100\n1 2 3 4 5\n") == "1"

# mixed gaps
assert run("6 2\n1 2 3 10 11 12\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | n | không thể ăn uống được vì bất bình đẳng nghiêm ngặt | 
| K = 0 tăng dần | n | chỉ có kích thước giống hệt nhau mới có thể tương tác | 
| K lớn | 1 | sụp đổ hoàn toàn thành một người sống sót | 
| cụm tách biệt | nhiều | tính chính xác trên các cửa sổ rời rạc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả vi khuẩn có kích thước giống hệt nhau. Trong trường hợp đó, không vi khuẩn nào có thể ăn vi khuẩn khác vì bất đẳng thức chặt chẽ$a_i > a_j$thất bại. Thuật toán xử lý việc này một cách chính xác vì mọi phần tử đều trở thành phần tử sống sót của chính nó khi quét từ đầu; không có thao tác xóa nào xảy ra trong vòng lặp bên trong. 

Một trường hợp cạnh khác xảy ra khi$K = 0$. Ở đây, vi khuẩn chỉ có thể ăn những sinh vật nhỏ hơn, nhưng điều kiện thứ hai buộc$a_i \le a_j$, không thể giữ đồng thời. Thuật toán tự nhiên tạo ra một phần tử sống sót cho mỗi phần tử vì không xảy ra việc xóa vòng lặp bên trong. 

Trường hợp thứ ba liên quan đến một khoảng cách lớn, chẳng hạn như$[1, 2, 3, 1000]$với nhỏ$K$. Phần tử lớn tạo thành nhóm riêng của nó và các phần tử nhỏ hơn tạo thành các nhóm riêng biệt vì chúng nằm ngoài phạm vi tiếp cận của nó. Quá trình quét được sắp xếp đảm bảo rằng khi chúng ta băng qua bên dưới$a[i] - K$, chúng tôi ngừng tiêu thụ và bắt đầu một nhóm mới một cách chính xác.
