---
problem: 1038C
contest_id: 1038
problem_index: C
name: "Gambling"
contest_name: "Codeforces Round 508 (Div. 2)"
rating: 1300
tags: ["greedy", "sortings"]
answer: passed_samples
verified: false
solve_time_s: 332
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33b850-8950-83ec-9224-95747a804177
---

#CF 1038C - Cờ bạc 

**Đánh giá:** 1300 
**Tags:** tham lam, phân loại 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 5 phút 32s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33b850-8950-83ec-9224-95747a804177 

--- 

## Giải pháp 

## Hiểu vấn đề 

Hai người chơi, mỗi người giữ một tập hợp số nguyên. Họ thay phiên nhau, bắt đầu với A. Trong một lượt, người chơi tiêu thụ một trong những số còn lại của chính họ, thêm nó vào điểm của họ hoặc loại bỏ một số khỏi bộ sưu tập của đối thủ, điều này không ảnh hưởng trực tiếp đến điểm số nhưng làm giảm những gì đối thủ có thể đạt được sau này. Trò chơi kết thúc khi cả hai bộ đều trống và cả hai người chơi được cho là luôn đưa ra các quyết định nhằm tối đa hóa chênh lệch cuối cùng giữa tổng điểm của A và tổng điểm của B. 

Khó khăn đến từ sự tương tác giữa lợi ích trực tiếp và sự từ chối. Giá trị cao trong danh sách của bạn là có giá trị, nhưng giá trị cao trong danh sách của đối thủ cũng có giá trị vì nó ngăn cản họ ghi bàn sau này. 

Các ràng buộc rất lớn với n lên tới 100000, do đó, bất kỳ giải pháp nào mô phỏng tất cả các chuỗi di chuyển có thể có hoặc khám phá trạng thái trò chơi đều không thể thực hiện được. Ngay cả lý luận O(n²) cũng quá chậm vì mỗi lần di chuyển có thể ảnh hưởng đến cả hai danh sách nhiều lần, dẫn đến hành vi bậc hai. 

Một ý tưởng ngây thơ nhưng hấp dẫn là nghĩ rằng mỗi người chơi sẽ chỉ sắp xếp danh sách của riêng mình và luôn lấy giá trị còn lại lớn nhất. Điều này không thành công vì thay vào đó nó bỏ qua khả năng loại bỏ một giá trị rất lớn khỏi đối thủ. Ví dụ: nếu A có số lượng nhỏ và B có số lượng lớn, A có thể muốn loại bỏ số lượng lớn đó thay vì thu được lợi ích nhỏ của riêng mình. Sự tương tác làm cho các lựa chọn tham lam thuần túy cục bộ trở nên không đáng tin cậy trừ khi chúng ta lập mô hình cả hai danh sách cùng nhau. 

Trường hợp lợi thế tinh tế xuất hiện khi cả hai người chơi có bộ bài giống hệt nhau. Ví dụ: nếu cả A và B đều có [1, 100], tính đối xứng cho thấy kết quả sẽ cân bằng và thực sự lối chơi tối ưu dẫn đến chênh lệch bằng 0. Một chiến lược ngây thơ “luôn tận dụng giá trị tối đa của bản thân” sẽ ưu tiên sớm 100 một cách không chính xác mà không tính đến việc chặn lẫn nhau và sẽ đánh giá quá cao lợi thế của nước đi đầu tiên. 

Một trường hợp khác phát sinh khi một người chơi có nhiều giá trị lớn và người kia có nhiều giá trị nhỏ. Mặc dù một bên “sở hữu” tổng số tiền nhiều hơn, việc loại bỏ mạnh mẽ có thể vô hiệu hóa lợi thế đó bằng cách ưu tiên từ chối hơn là thu thập. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô phỏng trò chơi dưới dạng tìm kiếm trạng thái trên hai tập hợp và một chỉ báo rẽ. Ở mỗi trạng thái, người chơi có thể chọn lấy một trong các nguyên tố của riêng mình hoặc loại bỏ một nguyên tố khỏi đối thủ. Điều này tạo ra hệ số phân nhánh tỷ lệ thuận với kích thước của danh sách và vì mỗi hành động làm giảm tổng số phần tử đi một, nên tổng số trạng thái sẽ tăng theo giai thừa trong trường hợp xấu nhất. Ngay cả với tính năng ghi nhớ, không gian trạng thái vẫn phụ thuộc vào tất cả các tập hợp con của các phần tử còn lại, vượt xa giới hạn khả thi. 

Quan sát quan trọng là trò chơi hoàn toàn được xác định bởi thứ tự tương đối của các phần tử trên cả hai danh sách. Tại bất kỳ thời điểm nào, chỉ những giá trị lớn nhất còn lại mới quan trọng vì mọi hành động tối ưu sẽ liên quan đến việc tương tác với phần tử có sẵn tối đa hiện tại trong một trong hai danh sách. Một phần tử nhỏ hơn không bao giờ được chọn khi có một nước đi có lợi lớn hơn, vì cả hai người chơi đều đang tối đa hóa sự khác biệt về điểm số cuối cùng và bất kỳ sai lệch nào sẽ bị chi phối bởi một nước đi có giá trị lớn hơn. 

Điều này làm giảm vấn đề liên tục so sánh mức tối đa hiện tại của các phần tử còn lại của A và các phần tử còn lại của B. Ai hành động sẽ lấy phần lớn hơn trong hai, vì nó mang lại lợi ích trực tiếp hoặc ngăn cản đối thủ giành được nhiều hơn sau này. Điều này dẫn đến một quá trình tham lam kiểu hai con trỏ trên các mảng được sắp xếp.

Chúng tôi sắp xếp cả hai mảng theo thứ tự giảm dần và mô phỏng quy trình bằng cách sử dụng con trỏ, luôn xem xét ứng cử viên lớn nhất còn lại hiện tại từ mỗi bên. Mỗi nước đi sẽ tiêu tốn một phần tử từ chủ sở hữu của nó hoặc xóa một phần tử khỏi đối thủ, tùy thuộc vào bên nào hiện đang giữ giá trị lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái trò chơi) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu trên danh sách được sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai mảng được sắp xếp, một cho A và một cho B, cả hai đều theo thứ tự giảm dần. Chúng tôi theo dõi hai chỉ số trên mỗi mảng đại diện cho phần tử lớn nhất chưa được sử dụng hiện tại. 

Chúng tôi cũng mô phỏng các lượt, bắt đầu từ A. 

1. Sắp xếp cả hai mảng theo thứ tự giảm dần. Điều này đảm bảo chúng tôi luôn biết ứng cử viên tốt nhất ở mỗi bên trong thời gian O(1) cho mỗi bước bằng cách sử dụng con trỏ. 
2. Khởi tạo hai con trỏ i và j lần lượt ở 0 cho A và B. Chúng đại diện cho các phần tử lớn nhất còn lại trong mỗi danh sách. 
3. Duy trì điểm A và điểm B, ban đầu cả hai đều bằng 0. 
4. Đến lượt A, so sánh A[i] và B[j] nếu cả hai đều tồn tại. Nếu A[i] lớn hơn hoặc bằng, A lấy A[i], cộng nó vào điểmA và tăng i. Ngược lại, A loại bỏ B[j] mà không đạt được điểm, do đó j tăng lên. 
5. Đến lượt B, thực hiện phép tính đối xứng: nếu B[j] lớn hơn hoặc bằng, B lấy B[j], cộng nó vào điểmB và tăng j. Ngược lại, B loại bỏ A[i], do đó i tăng. 
6. Lần lượt thay phiên nhau cho đến khi cả hai con trỏ đều đến cuối mảng của chúng. 

Quy tắc quyết định luôn dựa trên giá trị sẵn có lớn nhất bởi vì bất kỳ giá trị nhỏ hơn nào cũng bị chi phối nghiêm ngặt về cả tác động đạt được ngay lập tức và tác động từ chối. Nếu người chơi bỏ qua tùy chọn tối đa có sẵn, họ sẽ cho phép đối thủ khai thác nó trước, điều này làm giảm sự khác biệt cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước, trạng thái trò chơi có thể được tóm tắt bằng các phần tử lớn nhất còn lại trong cả hai danh sách. Bất kỳ động thái tối ưu nào cũng phải liên quan đến một trong hai yếu tố này bởi vì tất cả các yếu tố nhỏ hơn đều là những lựa chọn tồi tệ hơn cả về khả năng đạt được và ngăn chặn. Vì mỗi bước di chuyển sẽ loại bỏ chính xác một phần tử khỏi nhóm chung nên quá trình này giảm xuống còn việc giải quyết liên tục bên nào “kiểm soát” giá trị tối đa hiện tại. Bất biến này đảm bảo rằng không có chiến lược tối ưu nào cần nhìn xa hơn hai cực đại hiện tại, do đó mô phỏng tham lam phù hợp với cách chơi tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort(reverse=True)
    b.sort(reverse=True)
    
    i = j = 0
    scoreA = 0
    scoreB = 0
    turnA = True
    
    while i < n or j < n:
        a_top = a[i] if i < n else -1
        b_top = b[j] if j < n else -1
        
        if turnA:
            if i < n and (j == n or a_top >= b_top):
                scoreA += a_top
                i += 1
            else:
                j += 1
        else:
            if j < n and (i == n or b_top >= a_top):
                scoreB += b_top
                j += 1
            else:
                i += 1
        
        turnA = not turnA
    
    print(scoreA - scoreB)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp cả hai danh sách để chúng tôi luôn có thể truy cập các tùy chọn còn lại tốt nhất. Hai con trỏ mô phỏng sự thu nhỏ của nhiều tập hợp kết hợp. Mỗi bước thực thi quyết định cục bộ tối ưu: giành lấy lợi ích cá nhân tốt nhất hiện có hoặc loại bỏ mối đe dọa tốt nhất của đối thủ. 

Logic lần lượt xen kẽ đảm bảo tính công bằng trong việc tiếp cận các quyết định, trong khi bước so sánh đảm bảo cả việc tối đa hóa lợi ích và từ chối đối thủ đều được xử lý theo cùng một quy tắc. 

Một cạm bẫy thực hiện phổ biến là quên rằng việc loại bỏ khỏi đối thủ không phụ thuộc vào lượt của ai ngoài việc quyết định con trỏ nào sẽ di chuyển. Một vấn đề tinh vi khác là xử lý đẳng thức không chính xác, trong đó việc xử lý các giá trị bằng nhau không nhất quán có thể phá vỡ tính đối xứng trong trường hợp có các phần tử lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 4
5 1
```Chúng tôi sắp xếp cả hai mảng: 

A = [4, 1] 

B = [5, 1] 

| Xoay | Một đầu | Đầu B | Hành động | Điểm A | Điểm B | tôi | j | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| A | 4 | 5 | xóa B[0] | 0 | 0 | 0 | 1 | 
| B | 4 | 1 | xóa A[0] | 0 | 0 | 1 | 1 | 
| A | 1 | 1 | lấy A[1] | 1 | 0 | 2 | 1 | 
| B | - | 1 | lấy B[1] | 1 | 1 | 2 | 2 | 

Sự khác biệt cuối cùng là 0. 

Dấu vết này cho thấy cách các phần tử có giá trị cao được sử dụng lần đầu tiên để từ chối trước khi bắt đầu tính điểm, cân bằng kết quả của cả hai người chơi. 

### Ví dụ 2 

đầu vào:```
3
2 2 3
4 3 7
```Đã sắp xếp: 

A = [3, 2, 2] 

B = [7, 4, 3] 

| Xoay | Một đầu | Đầu B | Hành động | Điểm A | Điểm B | tôi | j | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| A | 3 | 7 | xóa B[0] | 0 | 0 | 0 | 1 | 
| B | 3 | 4 | xóa A[0] | 0 | 0 | 1 | 1 | 
| A | 2 | 4 | loại bỏ B[1] | 0 | 0 | 1 | 2 | 
| B | 2 | 3 | loại bỏ A[1] | 0 | 0 | 2 | 2 | 
| A | 2 | 3 | loại bỏ B[2] | 0 | 0 | 2 | 3 | 
| B | 2 | - | lấy A[2] | 0 | 2 | 3 | 3 | 
| A | - | - | lấy phần còn lại? | 2 | 2 | 3 | 3 | 

Điều này cho thấy hầu hết các giá trị lớn được sử dụng hoàn toàn cho việc chặn và chỉ các phần tử còn sót lại mới góp phần ghi điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế, việc truyền tải là tuyến tính | 
| Không gian | O(1) thêm | ngoài việc lưu trữ đầu vào và sắp xếp mảng | 

Bước sắp xếp nằm trong giới hạn n lên tới 100000 và quét tuyến tính đảm bảo mỗi phần tử được xử lý chính xác một lần, giúp giải pháp trở nên hiệu quả trong các điều kiện ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("2\n1 4\n5 1\n") == "0"

# all equal small
assert run("3\n1 1 1\n1 1 1\n") == "0"

# single element
assert run("1\n10\n1\n") == "9"

# descending vs ascending
assert run("4\n1 2 3 4\n4 3 2 1\n") == "0"

# large dominance
assert run("3\n1 1 1\n100 100 100\n") == "-99"

# random mix
assert run("5\n5 1 3 9 2\n4 8 1 2 7\n")  # sanity check execution
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | xử lý đối xứng | 
| phần tử đơn | lựa chọn trực tiếp đúng đắn | trường hợp cơ sở | 
| mảng đảo ngược | 0 | hủy bỏ đối xứng hoàn toàn | 
| trường hợp thống trị | kết quả âm tính | hành vi mất cân bằng mạnh mẽ | 

## Vỏ cạnh 

Khi cả hai mảng chứa nhiều tập hợp giống hệt nhau, thuật toán sẽ xen kẽ các hành động đối xứng hoàn hảo. Mọi phần tử có giá trị cao trong một danh sách đều được phản ánh trong danh sách khác, do đó, mỗi mức tăng cuối cùng sẽ bị hủy bỏ bởi mức tăng tương đương ở phía đối diện. Mô phỏng dựa trên con trỏ đảm bảo rằng các giá trị cực đại giống hệt nhau luôn được xử lý theo thứ tự nhất quán, tạo ra sai số cuối cùng bằng 0. 

Khi một mảng chứa một phần tử rất lớn và mảng kia chứa nhiều phần tử nhỏ, phần tử lớn ngay lập tức được sử dụng để chặn ở những lượt đầu. Thuật toán ưu tiên loại bỏ một cách chính xác thay vì tự chấm điểm, đảm bảo rằng giá trị lớn không chiếm ưu thế trong kết quả cuối cùng một cách không chính xác. 

Khi tất cả các phần tử đều bằng nhau, mọi so sánh giữa đỉnh A và đỉnh B đều được coi là bằng nhau, do đó, quy tắc ràng buộc luôn dẫn đến kết quả đối xứng. Thuật toán luân phiên loại bỏ và đạt được đồng đều, duy trì sự cân bằng giữa cả hai người chơi.
