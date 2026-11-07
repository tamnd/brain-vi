---
problem: 1352C
contest_id: 1352
problem_index: C
name: "K-th Not Divisible by n"
contest_name: "Codeforces Round 640 (Div. 4)"
rating: 1200
tags: ["binary search", "math"]
answer: passed_samples
verified: true
solve_time_s: 157
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e2bb2-e3e0-83ec-8dea-7fd34d7f3a5f
---

# CF 1352C - K-th Không chia hết cho n 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e2bb2-e3e0-83ec-8dea-7fd34d7f3a5f 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta đang xem xét một chuỗi các số nguyên dương được lọc trong đó mọi bội số của một số cố định đều bị loại bỏ. Thay vì làm việc với danh sách đầy đủ các số tự nhiên, hãy tưởng tượng việc đánh dấu tất cả các số chia hết cho`n`như bị cấm, sau đó nén những gì còn lại thành một chuỗi rõ ràng. Nhiệm vụ là xác định số nào xuất hiện ở vị trí`k`của chuỗi nén này. 

Khó khăn chính là trình tự được lọc không được đưa ra một cách rõ ràng. Đối với các giá trị lớn, chuỗi cực kỳ dài, vì vậy chúng ta cần một cách để chuyển thẳng đến câu trả lời mà không cần liệt kê từng phần tử một. 

Các ràng buộc làm cho một mô phỏng đơn giản không thể thực hiện được. Vì cả hai`n`Và`k`có thể lên tới một tỷ và có thể có tới 1000 trường hợp thử nghiệm, bất kỳ phương pháp nào lặp qua các số nguyên và kiểm tra tính chia hết sẽ yêu cầu tới 10^9 thao tác cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Điều đó vượt xa những gì có thể chạy trong một giây. 

Trường hợp cạnh tinh tế xuất hiện khi`n`là nhỏ. Ví dụ, nếu`n = 2`Và`k = 1`, câu trả lời là`1`, nhưng nếu đếm nhầm các chỉ số không chính xác và bắt đầu từ 0 hoặc bao gồm bội số của`n`, sự dịch chuyển từng bước một nhanh chóng lan truyền. Một trường hợp cạnh khác là khi`n`rất lớn, chẳng hạn như`n = 10^9`. Trong trường hợp này, hầu hết mọi số đều hợp lệ ngoại trừ một tỷ bước, vì vậy câu trả lời cực kỳ gần với`k`và việc lọc ngây thơ sẽ lãng phí hầu hết mọi công việc đối với những con số không nên xóa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp qua các số nguyên dương, duy trì bộ đếm có bao nhiêu số không chia hết cho`n`, và dừng lại khi đạt đến`k`-số hợp lệ thứ Điều này đơn giản và chính xác vì nó mô phỏng trực tiếp định nghĩa của chuỗi. Tuy nhiên, chi phí của nó phụ thuộc vào số lượng chúng tôi quét. Trong trường hợp xấu nhất, nếu`n = 2`, chỉ một nửa số là hợp lệ nên có thể chúng ta cần kiểm tra đại khái`2k`số cho mỗi trường hợp thử nghiệm. Với`k`lên tới 10^9, điều này trở nên hoàn toàn không thể. 

Quan sát quan trọng là ngừng suy nghĩ về trình tự tăng dần và thay vào đó suy luận theo khối. Trong mỗi khối của`n`chính xác là các số nguyên liên tiếp`n - 1`trong số chúng hợp lệ vì chỉ có một số trong mỗi khối chia hết cho`n`. Điều này biến vấn đề thành việc đếm các khối đầy đủ và sau đó xử lý phần còn lại bên trong một phần khối. 

Khi cấu trúc này được nhận dạng, chúng ta có thể tính toán có bao nhiêu khối kích thước đầy đủ`n`là cần thiết để đạt được`k`-số hợp lệ, sau đó điều chỉnh các bội số bị thiếu đã bị bỏ qua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu nhóm đầy đủ`n`số lượng là cần thiết để trang trải`k`những con số hợp lệ. Mỗi nhóm đóng góp`n - 1`số hợp lệ nên ta chia`k`qua`n - 1`. Điều này cho chúng ta biết chúng ta có thể lắp bao nhiêu khối đầy đủ trước khi đến vị trí mục tiêu. 
2. Tính phần còn lại của các số hợp lệ cần thiết sau các khối đầy đủ này. Phần còn lại này xác định câu trả lời nằm ở khối tiếp theo bao xa. 
3. Chuyển đổi số khối thành vị trí số thực tế bằng cách nhân số khối đầy đủ với`n`. Điều này đưa ra một vị trí cơ sở trong dòng số nguyên ban đầu chiếm bội số bị bỏ qua. 
4. Nếu số dư khác 0, chúng ta tiến lên theo số dư đó để tìm đúng số hợp lệ. Nếu phần còn lại bằng 0, chúng ta đang ở chính xác ranh giới và phải lùi lại một bước để tránh chạm vào bội số của`n`. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi phân đoạn liên tiếp của`n`số nguyên chứa chính xác`n - 1`các số hợp lệ và chính xác một số bị loại trừ ở một độ lệch cố định (bội số của`n`). Bằng cách nhóm dãy số thành các khối có kích thước bằng nhau, chúng ta biến quá trình loại bỏ rải rác thành một mẫu tuần hoàn. Tính tuần hoàn này đảm bảo rằng việc đếm các số hợp lệ trên toàn cầu tương đương với việc đếm các khối đầy đủ cộng với sự điều chỉnh cục bộ bên trong một khối duy nhất, do đó không có thông tin nào bị mất khi nén chuỗi thành số học khối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())

        # number of full blocks of size (n-1) valid numbers
        blocks = (k - 1) // (n - 1)

        # position after accounting for removed multiples
        ans = blocks * n + (k - blocks * (n - 1))

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp mã hóa lý luận khối. biểu hiện`(k - 1) // (n - 1)`tính toán có bao nhiêu hoàn thành`(n - 1)`-các nhóm số hợp lệ có kích thước phù hợp trước khi tiếp cận`k`. nhân với`n`chuyển các nhóm đó trở lại trục số ban đầu, lắp lại các bội số bị bỏ qua. 

Điều chỉnh cuối cùng sẽ thêm phần bù còn lại vào khối hiện tại. Phép trừ 1 trong tính toán khối là rất quan trọng để tránh các lỗi sai sót khi`k`chính xác là chia hết cho`n - 1`, trong đó câu trả lời sẽ xuất hiện ngay trước bội số của`n`, không có trên đó. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để xem cấu trúc khối thay thế bảng liệt kê rõ ràng như thế nào. 

Ví dụ đầu tiên:`n = 3, k = 7`. 

| Bước | khối | logic dư | kết quả | 
| --- | --- | --- | --- | 
| khối tính toán | 3 | (7-1)//2 = 3 | 3 khối đầy đủ | 
| vị trí cơ sở | 3 | 3 * 3 = 9 | điểm bắt đầu | 
| phần còn lại | 1 | 7 - 6 = 1 | di chuyển 1 bước | 
| câu trả lời cuối cùng | - | - | 10 | 

Điều này cho thấy sau khi tính toán đủ ba khối có kích thước 3 (mỗi khối đóng góp 2 số hợp lệ), chúng ta tiến đến gần 9 rồi điều chỉnh vào bên trong khối tiếp theo. 

Ví dụ thứ hai:`n = 4, k = 12`. 

| Bước | khối | logic dư | kết quả | 
| --- | --- | --- | --- | 
| khối tính toán | 4 | (12-1)//3 = 3 | 3 khối đầy đủ | 
| vị trí cơ sở | 3 | 3 * 4 = 12 | điểm bắt đầu | 
| phần còn lại | 3 | 12 - 9 = 3 | di chuyển 3 bước | 
| câu trả lời cuối cùng | - | - | 15 | 

Điều này xác nhận rằng các khối đầy đủ đóng góp vào cấu trúc, trong khi phần còn lại xử lý việc định vị cục bộ bên trong một khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi truy vấn sử dụng một số phép toán số học không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng phù hợp trong giới hạn vì thậm chí 1000 trường hợp thử nghiệm chỉ yêu cầu số học số nguyên đơn giản, không lặp lại trên phạm vi lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        blocks = (k - 1) // (n - 1)
        ans = blocks * n + (k - blocks * (n - 1))
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("6\n3 7\n4 12\n2 1000000000\n7 97\n1000000000 1000000000\n2 1\n") == \
"10\n15\n1999999999\n113\n1000000001\n1"

# custom cases
assert run("1\n3 1\n") == "1", "first valid number"
assert run("1\n3 2\n") == "2", "simple progression"
assert run("1\n3 3\n") == "4", "skip multiple"
assert run("1\n10 9\n") == "10", "boundary before multiple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 | 1 | trường hợp không bội nhỏ nhất | 
| 3 3 | 4 | vượt qua nhiều ranh giới | 
| 10 9 | 10 | cạnh ngay trước số bị loại trừ | 

## Vỏ cạnh 

Khi nào`k`rất nhỏ, chẳng hạn như`k = 1`, công thức vẫn hoạt động vì`(k - 1) // (n - 1)`trở thành 0, đặt câu trả lời đúng ở số hợp lệ đầu tiên, luôn luôn là`1`. 

Khi`k`chính xác là bội số của`n - 1`, quá trình tính toán sẽ diễn ra chính xác tại ranh giới của một khối. Trong tình huống đó, phép nhân với`n`đảm bảo chúng tôi bỏ qua các bội số bị loại trừ một cách chính xác và việc điều chỉnh phần dư tránh bước vào bội số bị cấm. 

Khi`n`là cực kỳ lớn, kích thước khối trở nên thưa thớt đến mức hầu hết mọi số đều hợp lệ. Công thức suy biến rõ ràng thành trả về`k`hoặc`k + 1`tùy thuộc vào sự căn chỉnh, khớp với cấu trúc trình tự trực quan mà không cần xử lý đặc biệt.
