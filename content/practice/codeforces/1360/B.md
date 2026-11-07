---
problem: 1360B
contest_id: 1360
problem_index: B
name: "Honest Coach"
contest_name: "Codeforces Round 644 (Div. 3)"
rating: 800
tags: ["greedy", "sortings"]
answer: passed_samples
verified: true
solve_time_s: 218
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e3de0-0334-83ec-9ad2-5a24d5b4b9f9
---

#CF 1360B - Huấn luyện viên trung thực 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e3de0-0334-83ec-9ad2-5a24d5b4b9f9 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp, chúng tôi nhận được một danh sách sức mạnh của vận động viên và chúng tôi phải chia các vận động viên thành hai nhóm không trống sao cho một nhóm đóng góp giá trị tối đa và nhóm kia đóng góp giá trị tối thiểu và sự khác biệt tuyệt đối giữa hai giá trị đó càng nhỏ càng tốt. 

Một cách hữu ích để diễn đạt lại mục tiêu là chúng ta đang cố gắng chia nhiều tập giá trị thành hai phần sao cho ranh giới giữa hai phần, về mặt không gian giá trị, càng chặt chẽ càng tốt. Một nhóm sẽ chứa một số tập hợp con các điểm mạnh và chúng ta chỉ quan tâm đến mức tối đa của nhóm đó, trong khi nhóm còn lại chỉ đóng góp mức tối thiểu. Tư cách thành viên thực tế của các thành phần còn lại trong mỗi nhóm không ảnh hưởng đến điểm số ngoài việc duy trì hai thái cực đó. 

Những hạn chế là nhỏ. Mỗi test case có tối đa 50 phần tử và có tối đa 1000 test case. Điều này ngay lập tức loại trừ mọi chiến lược phân vùng theo cấp số nhân cho mỗi trường hợp thử nghiệm. Ngay cả một giải pháp bậc hai cho mỗi trường hợp thử nghiệm cũng an toàn, vì 50 bình phương chỉ là 2500 phép tính. 

Điểm tinh tế chính là việc phân nhóm không bị ràng buộc bởi vị trí hoặc thứ tự, chỉ bởi lựa chọn giá trị. Điều này có nghĩa là bất kỳ lý do nào dựa trên tính kề cận trong mảng đầu vào đều không liên quan và chỉ có thứ tự sắp xếp của các giá trị mới quan trọng. 

Một lỗi phổ biến là cố gắng gán các phần tử một cách tham lam mà không sắp xếp. Ví dụ: người ta có thể thử xây dựng hai tập hợp bằng cách luân phiên gán hoặc chia xung quanh điểm trung bình của các chỉ số. Điều đó không thành công vì tính tối ưu phụ thuộc vào mức độ gần nhau về mặt số chứ không phải thứ tự ban đầu. 

Một dạng lỗi khác xuất hiện khi giả định mức phân chia tốt nhất là xung quanh giá trị trung bình. Điều này không phải lúc nào cũng đúng vì các khoảng trống trùng lặp hoặc không đồng đều có thể tạo ra một đường cắt khác tối ưu. Ví dụ, nếu các giá trị là`[1, 2, 100, 101]`, việc chia từ 2 đến 100 rõ ràng là tối ưu, nhưng nếu các giá trị là`[1, 2, 3, 100]`, mức phân chia tốt nhất là từ 3 đến 100, không xoay quanh chỉ số trung vị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi phân vùng có thể có của mảng thành hai tập hợp không trống. Đối với mỗi phân vùng, hãy tính giá trị tối đa của một bộ và giá trị tối thiểu của bộ kia, sau đó đánh giá chênh lệch tuyệt đối. có`2^n`các phân vùng có thể có, và với mỗi phân vùng chúng ta sẽ cần tới`O(n)`tính cực đại và cực tiểu, dẫn đến`O(n·2^n)`độ phức tạp cho mỗi trường hợp thử nghiệm. Với`n = 50`, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là giá trị chỉ phụ thuộc vào ranh giới giữa hai bộ theo thứ tự được sắp xếp. Sau khi sắp xếp mảng, chúng ta có thể nghĩ đến việc chọn một điểm phân chia để xác định giá trị nào thuộc nhóm bên trái và giá trị nào thuộc nhóm bên phải. Bất kỳ phân vùng tối ưu nào cũng có thể được chuyển đổi thành phân vùng trong đó tất cả các phần tử trong nhóm A nhỏ hơn hoặc bằng tất cả các phần tử trong nhóm B. Nếu thuộc tính này bị vi phạm, việc hoán đổi các phần tử sẽ chỉ cải thiện hoặc duy trì mục tiêu. 

Sau khi sắp xếp, câu trả lời của thí sinh sẽ chuyển sang kiểm tra tất cả các cặp liền kề trong mảng đã sắp xếp. Nếu chúng ta phân chia giữa các vị trí`i`Và`i+1`, thì sự đóng góp tốt nhất có thể là`|s[i] - s[i+1]|`, vì nhóm A có thể kết thúc tại`s[i]`và nhóm B có thể bắt đầu lúc`s[i+1]`. Bất kỳ bài tập nào khác có khoảng cách lớn hơn sẽ chỉ làm tăng sự khác biệt. 

Do đó, bài toán tập trung vào việc tìm sự khác biệt nhỏ nhất giữa các phần tử liên tiếp trong mảng đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc danh sách điểm mạnh của test case hiện tại. Mục đích là chuẩn bị dữ liệu sao cho các so sánh phản ánh mức độ gần nhau của giá trị hơn là thứ tự đầu vào. 
2. Sắp xếp mảng theo thứ tự không giảm. Bước này rất cần thiết vì nó căn chỉnh tất cả các phần phân chia có khả năng tối ưu cạnh nhau trong không gian giá trị. Nếu không sắp xếp, sự khác biệt liền kề không có ý nghĩa. 
3. Khởi tạo một biến`best`với giá trị lớn. Điều này sẽ lưu trữ sự khác biệt nhỏ nhất được tìm thấy giữa các ranh giới phân chia hợp lệ. 
4. Lặp lại mảng đã được sắp xếp từ chỉ số 1 đến n−1. Tại mỗi vị trí, tính toán sự khác biệt giữa phần tử hiện tại và phần tử trước đó. Điều này thể hiện chi phí của việc chia mảng giữa hai giá trị này. 
5. Cập nhật`best`bất cứ khi nào một sự khác biệt nhỏ hơn được tìm thấy. Điều này đảm bảo chúng tôi luôn theo dõi sự tách biệt chặt chẽ nhất có thể giữa hai nhóm. 
6. Sau khi xử lý tất cả các cặp liền kề, xuất ra`best`như câu trả lời cho trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, bất kỳ phân vùng hợp lệ nào cũng có thể được coi là chọn một ngưỡng phân tách mảng thành hai phần không trống. Giá trị của phân vùng chỉ phụ thuộc vào phần tử lớn nhất ở bên trái và phần tử nhỏ nhất ở bên phải. Khoảng cách nhỏ nhất có thể có giữa một cặp như vậy phải xảy ra giữa hai phần tử liên tiếp theo thứ tự được sắp xếp, bởi vì bất kỳ bước nhảy lớn hơn nào sẽ bỏ qua cặp ứng cử viên gần hơn. Vì vậy, việc quét những khác biệt liền kề là đủ để đảm bảo sự phân chia tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = list(map(int, input().split()))
    
    s.sort()
    
    best = float('inf')
    for i in range(1, n):
        best = min(best, s[i] - s[i - 1])
    
    print(best)
```Giải pháp trước tiên sắp xếp từng trường hợp thử nghiệm, đây là phép biến đổi cấu trúc làm cho vấn đề trở thành một chiều trong không gian giá trị. Vòng lặp trên các cặp liền kề mã hóa trực tiếp các điểm phân chia có ý nghĩa duy nhất. 

Phép trừ`s[i] - s[i - 1]`là an toàn vì mảng đã được sắp xếp, do đó chênh lệch luôn không âm và thể hiện chi phí chính xác của việc phân chia giữa hai phần tử đó. 

Việc khởi tạo của`best`với vô cực sẽ tránh mọi xử lý trường hợp đặc biệt cho lần so sánh đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 2 6 4
```Mảng được sắp xếp:`[1, 2, 3, 4, 6]`| tôi | Cặp | Sự khác biệt | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | (1,2) | 1 | 1 | 
| 2 | (2,3) | 1 | 1 | 
| 3 | (3,4) | 1 | 1 | 
| 4 | (4,6) | 2 | 1 | 

Khoảng cách nhỏ nhất là 1, tương ứng với nhiều phép chia hợp lệ. Điều này xác nhận rằng phân vùng tối ưu chỉ phụ thuộc vào các giá trị liền kề gần nhất. 

### Ví dụ 2 

đầu vào:```
4
7 9 3 1
```Mảng được sắp xếp:`[1, 3, 7, 9]`| tôi | Cặp | Sự khác biệt | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | (1,3) | 2 | 2 | 
| 2 | (3,7) | 4 | 2 | 
| 3 | (7,9) | 2 | 2 | 

Câu trả lời tối ưu là 2, đạt được bằng cách phân chia giữa 1 và 3 hoặc từ 7 đến 9. Điều này cho thấy có thể tồn tại nhiều ranh giới tối ưu và thuật toán nắm bắt chính xác mức tối thiểu trong số đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp chiếm ưu thế, quét tuyến tính theo sau | 
| Không gian | O(1) thêm không gian | Chỉ một số biến được sử dụng ngoài bộ lưu trữ đầu vào | 

Được cho`n ≤ 50`Và`t ≤ 1000`, tổng công việc là tầm thường dưới những ràng buộc. Ngay cả 1000 loại kích thước 50 trong trường hợp xấu nhất cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = list(map(int, input().split()))
        s.sort()
        best = float('inf')
        for i in range(1, n):
            best = min(best, s[i] - s[i - 1])
        out.append(str(best))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("""5
5
3 1 2 6 4
6
2 1 3 2 4 3
4
7 9 3 1
2
1 1000
3
100 150 200
""") == """1
0
2
999
50"""

# custom cases
assert run("""1
2
10 20
""") == "10"

assert run("""1
5
5 5 5 5 5
""") == "0"

assert run("""1
6
1 100 101 102 200 300
""") == "1"

assert run("""1
4
1 2 100 101
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 20`|`10`| kích thước tối thiểu và phân chia đơn giản | 
| tất cả các giá trị bằng nhau |`0`| xử lý khoảng cách bằng không | 
| cụm hỗn hợp |`1`| cặp gần nhất qua ranh giới | 
| cụm tách biệt |`1`| đảm bảo suy luận liền kề đúng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị giống hệt nhau. Đối với đầu vào`[5, 5, 5, 5]`, việc sắp xếp giữ cho nó không thay đổi và mọi khác biệt liền kề đều bằng 0. Thuật toán trả về 0 một cách chính xác vì bất kỳ sự phân chia nào cũng tạo ra mức tối đa và tối thiểu giống hệt nhau giữa các nhóm. 

Một trường hợp khác là khi sự phân chia tốt nhất không ở gần điểm cực trị mà ở giữa cấu trúc cụm, chẳng hạn như`[1, 100, 101, 102, 200]`. Việc sắp xếp cho cùng một trình tự và khoảng cách liền kề nhỏ nhất nằm giữa`100`Và`101`, thể hiện chính xác ranh giới phân vùng tối ưu. 

Trường hợp cuối cùng là kích thước đầu vào nhỏ nhất có thể`n = 2`. Thuật toán thực hiện một so sánh duy nhất và trả về sự khác biệt giữa hai phần tử duy nhất, đây là phép phân chia hợp lệ duy nhất.
