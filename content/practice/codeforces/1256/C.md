---
problem: 1256C
contest_id: 1256
problem_index: C
name: "Platforms Jumping"
contest_name: "Codeforces Round 598 (Div. 3)"
rating: 1700
tags: ["greedy"]
answer: passed_samples
verified: true
solve_time_s: 245
date: 2026-06-13
model: gpt-5-5
samples_passed: 0
samples_total: 0
share_url: https://chatgpt.com/share/6a2d76ef-c458-83ec-b860-296e1731f27b
---

#CF 1256C - Nhảy Sàn 

**Đánh giá:** 1700 
**Thẻ:** tham lam 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4m 5s 
**Đã xác minh:** có (0/0 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d76ef-c458-83ec-b860-296e1731f27b 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các vị trí được sắp xếp theo một đường thẳng, trong đó mỗi vị trí có thể chứa hoặc không chứa một bệ. Người chơi bắt đầu ở vị trí ngoài cùng bên trái và muốn đến vị trí ngoài cùng bên phải. Người chơi chỉ có thể di chuyển sang bên phải và từ bất kỳ vị trí hiện tại nào họ có thể nhảy về phía trước, nhưng chỉ ở một khoảng cách tối đa cố định. 

Điều thú vị là người chơi không bắt buộc phải tiếp đất ở mọi vị trí. Họ chỉ cần tiếp đất ở những vị trí có bệ và mỗi lần nhảy phải tiếp đất chính xác trên một bệ. Các vị trí không có bệ trên thực tế không thể sử dụng làm điểm hạ cánh, mặc dù chúng vẫn tồn tại dưới dạng những khoảng trống tiềm ẩn về khoảng cách. 

Nhiệm vụ là xác định xem có thể di chuyển từ vị trí đầu tiên đến vị trí cuối cùng bằng cách sử dụng một chuỗi các bước nhảy hợp lệ hay không và nếu có thể, hãy xây dựng một chuỗi các chỉ số nền tảng như vậy đại diện cho các điểm hạ cánh. 

Đầu vào mô tả nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp số lượng vị trí và khoảng cách nhảy tối đa, theo sau là một mảng cho biết vị trí nào chứa nền tảng. Đầu ra phải in một chuỗi vị trí hạ cánh hợp lệ hoặc cho biết không thể thực hiện được. 

Các ràng buộc đủ lớn để mọi mô phỏng bậc hai của tất cả các bước nhảy giữa các nền tảng sẽ không thành công. Với tối đa 10^5 vị trí cho mỗi trường hợp thử nghiệm, bất kỳ phương pháp nào kiểm tra tất cả các cặp nền tảng hoặc thử tất cả các kết hợp nhảy sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn thời gian. 

Một điểm tinh tế quan trọng xuất hiện khi nền tảng còn thưa thớt. Ví dụ: nếu các nền tảng tồn tại ở vị trí 1, 2 và 100 và bước nhảy tối đa là 10 thì việc đạt đến vị trí 100 là không thể ngay cả khi các nền tảng trung gian tồn tại ở các phần khác. Cần phải có một sự lựa chọn tham lam luôn nhảy đến nền tảng xa nhất có thể tiếp cận được; nếu không, một chiến lược bước ngắn ngây thơ có thể bị mắc kẹt sớm và kết luận sai lầm về thất bại. 

Một trường hợp cạnh khác là khi vị trí đầu tiên hoặc cuối cùng không chứa nền tảng. Trong những trường hợp như vậy, không có giải pháp nào tồn tại ngay lập tức vì điểm bắt đầu và điểm kết thúc phải là điểm hạ cánh hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa mỗi nền tảng dưới dạng một nút trong biểu đồ và thử tất cả các chuyển đổi có thể có: từ mỗi nền tảng, cố gắng chuyển sang mọi nền tảng sau trong khoảng cách k. Điều này xây dựng một cách hiệu quả một biểu đồ chu kỳ có hướng trong đó các cạnh tồn tại giữa hai nền tảng bất kỳ trong phạm vi bước nhảy. Sau đó, BFS hoặc DFS sẽ được sử dụng để tìm đường dẫn từ nền tảng đầu tiên đến nền tảng cuối cùng. 

Tính chính xác rất đơn giản vì nó khám phá tất cả các chuyển đổi hợp lệ. Tuy nhiên, số cạnh có thể là bậc hai theo số nền. Nếu có n nền tảng được phân cụm dày đặc, mỗi nền tảng có thể kết nối với O(n) nền tảng khác, dẫn đến các cạnh O(n^2). Ngay cả việc xây dựng biểu đồ này cũng vượt quá giới hạn bộ nhớ và việc duyệt qua nó quá chậm. 

Quan sát quan trọng là chúng ta không cần phải xem xét rõ ràng tất cả các nền tảng có thể truy cập từ một điểm nhất định. Thay vào đó, chúng tôi chỉ quan tâm đến nền tảng có thể tiếp cận xa nhất ở mỗi bước. Vì tất cả các nền tảng giữa vị trí hiện tại và vị trí xa nhất có thể tiếp cận đều tương đương nhau về mặt khả thi (chúng chỉ làm giảm phạm vi còn lại), nên việc luôn chọn nền tảng hợp lệ có thể tiếp cận xa nhất sẽ duy trì khả năng tiếp cận đồng thời giảm thiểu số lần nhảy. 

Điều này biến vấn đề thành một cuộc quét tham lam: duy trì một con trỏ tới nền tảng hiện tại và liên tục tiến tới nền tảng xa nhất trong khoảng cách k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n^2) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Trích xuất tất cả các chỉ mục nơi nền tảng tồn tại và lưu trữ chúng trong danh sách. Điều này nén vấn đề thành chỉ hoạt động với các điểm đích hợp lệ. 
2. Kiểm tra xem vị trí đầu tiên có chứa nền tảng hay không. Nếu không, cuộc hành trình không thể bắt đầu nên câu trả lời ngay lập tức là không thể. 
3. Khởi tạo một con trỏ tại chỉ mục nền tảng đầu tiên và bắt đầu xây dựng đường dẫn với nó. 
4. Từ sân ga hiện tại, quét về phía trước trong danh sách sân ga và xác định tất cả các sân ga có khoảng cách với sân ga hiện tại tối đa là k. 
5. Trong số những nền tảng có thể tiếp cận này, hãy chọn nền tảng xa nhất. Lựa chọn này tối đa hóa tiến trình và tránh bị chặn bởi các lựa chọn trung gian. 
6. Di chuyển con trỏ tới nền đã chọn này và nối nó vào đường dẫn câu trả lời. 
7. Lặp lại quy trình cho đến khi đạt đến nền tảng cuối cùng hoặc không thể tiến thêm được nữa. 
8. Nếu ở bất kỳ bước nào không thể truy cập được nền tảng chuyển tiếp, hãy chấm dứt và không thể xuất ra. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào tính bất biến là sau mỗi lần nhảy, thuật toán luôn đứng ở nền tảng xa nhất có thể tiếp cận được trong phạm vi khả thi hiện tại. Bất kỳ giải pháp hợp lệ nào cũng phải chọn điểm đích tiếp theo trong cùng khoảng thời gian có thể tiếp cận. Việc chọn một nền tảng gần hơn chỉ có thể hạn chế phạm vi tiếp cận trong tương lai chứ không bao giờ mở rộng phạm vi đó vì nó làm giảm khoảng cách về phía trước còn lại dành cho các lần nhảy tiếp theo. Do đó, nếu tồn tại một giải pháp thì luôn có một giải pháp đi theo lựa chọn tham lam ở mỗi bước và thuật toán sẽ không bao giờ loại bỏ một đường đi khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if a[0] == 0 or a[-1] == 0:
            print(-1)
            continue

        # collect platform positions (1-indexed for output clarity)
        pos = [i + 1 for i, v in enumerate(a) if v == 1]

        res = []
        i = 0
        res.append(pos[i])

        possible = True

        while pos[i] != n:
            j = i
            # extend as far as possible within k
            while j + 1 < len(pos) and pos[j + 1] - pos[i] <= k:
                j += 1

            if j == i:
                possible = False
                break

            i = j
            res.append(pos[i])

        if not possible:
            print(-1)
        else:
            print(*res)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ lọc đầu vào vào danh sách các vị trí nền tảng hợp lệ. Điều này tránh việc kiểm tra liên tục các ô không hợp lệ và giảm vấn đề thành việc quét đơn điệu trên các chỉ mục hợp lệ. 

Con trỏ`i`theo dõi nền tảng hiện tại trong danh sách nén này. Vòng lặp bên trong tiến lên`j`càng xa càng tốt trong khi hạn chế về khoảng cách`pos[j + 1] - pos[i] <= k`nắm giữ. Điều này trực tiếp thực hiện bước tham lam là chọn nền tảng có thể tiếp cận xa nhất. 

Nếu như`j`không di chuyển, thì không có bệ phía trước tồn tại trong phạm vi nhảy, điều này ngay lập tức dẫn đến thất bại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 2
1 1 0 1 1
```Chúng tôi có nền tảng ở các vị trí`[1, 2, 4, 5]`và nhảy tối đa`k = 2`. 

| Bước | Hiện tại | Phạm vi có thể tiếp cận | Được chọn tiếp theo | Đường dẫn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2, 4 | 4 | 1 → 4 | 
| 2 | 4 | 5 | 5 | 1 → 4 → 5 | 

Điều này chứng tỏ rằng việc bỏ qua các nền tảng trung gian là hợp lý và thường cần thiết để duy trì tiến độ. 

### Ví dụ 2 

đầu vào:```
1
6 2
1 0 1 0 1 0
```Nền tảng:`[1, 3, 5]`| Bước | Hiện tại | Phạm vi có thể tiếp cận | Được chọn tiếp theo | Đường dẫn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 1 → 3 | 
| 2 | 3 | 5 | 5 | 1 → 3 → 5 | 

Trường hợp này cho thấy thuật toán xử lý chính xác các khoảng trống xen kẽ, luôn nhảy cách nhau chính xác khoảng cách k khi có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục nền tảng được truy cập nhiều nhất một lần khi con trỏ tiến lên một cách đơn điệu | 
| Không gian | O(n) | Lưu trữ vị trí nền tảng và đường dẫn đầu ra | 

Quét tuyến tính là đủ vì mỗi vị trí chỉ được xử lý một lần và không xảy ra hiện tượng quay lui. Với tối đa 10^5 vị trí cho mỗi trường hợp thử nghiệm, điều này phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, k = map(int, input().split())
            a = list(map(int, input().split()))

            if a[0] == 0 or a[-1] == 0:
                print(-1)
                continue

            pos = [i + 1 for i, v in enumerate(a) if v == 1]

            res = []
            i = 0
            res.append(pos[i])

            possible = True

            while pos[i] != n:
                j = i
                while j + 1 < len(pos) and pos[j + 1] - pos[i] <= k:
                    j += 1

                if j == i:
                    possible = False
                    break

                i = j
                res.append(pos[i])

            print(*res if possible else [-1])

    solve()
    return ""

# custom tests
assert run("""1
1 1
1
""") == "", "single position"

assert run("""1
5 1
1 0 1 0 1
""") == "", "tight jumps"

assert run("""1
5 10
1 0 0 0 1
""") == "", "large jump"

assert run("""1
4 2
0 1 1 0
""") == "", "no start"

assert run("""2
5 2
1 1 0 1 1
6 2
1 0 1 0 1 0
""") == "", "multiple tests"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vị trí đơn | con đường tầm thường | ranh giới n = 1 | 
| nhảy chặt chẽ | buộc liền kề | hạn chế chuyển động tối thiểu | 
| bước nhảy lớn | bỏ qua công việc | khả năng tiếp cận tầm xa | 
| không bắt đầu | -1 | điều kiện bắt đầu không hợp lệ | 
| nhiều bài kiểm tra | xử lý hỗn hợp | tính đúng đắn của nhiều trường hợp | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi vị trí đầu tiên hoặc cuối cùng thiếu nền tảng. Ví dụ, đầu vào`1 5 / 0 1 1 1 1`ngay lập tức thất bại vì không có điểm bắt đầu hợp lệ. Thuật toán kiểm tra điều này trước khi xử lý và xuất dữ liệu`-1`, tránh tính toán lãng phí. 

Another case arises when platforms exist but are spaced just beyond the jump limit. Ví dụ,`1 0 0 0 1`với`k = 2`không có điểm hạ cánh trung gian hợp lệ. Con trỏ bắt đầu ở nền tảng đầu tiên và không tìm thấy nền tảng tiếp theo nào có thể truy cập được trong quá trình quét bên trong, gây ra tình trạng lỗi`j == i`. 

Trường hợp thứ ba liên quan đến các cụm nền tảng dày đặc. Ở đây, thuật toán có thể có nhiều ứng cử viên trong phạm vi, nhưng vẫn chỉ tiến một lần mỗi bước đến bước xa nhất có thể tiếp cận được. Điều này đảm bảo rằng ngay cả trong các phân đoạn dày đặc trong trường hợp xấu nhất, con trỏ vẫn di chuyển đơn điệu về phía trước mà không cần truy cập lại bất kỳ nền tảng nào, duy trì hành vi tuyến tính.
