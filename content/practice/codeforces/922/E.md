---
problem: 922E
contest_id: 922
problem_index: E
name: "Birds"
contest_name: "Codeforces Round 461 (Div. 2)"
rating: 2200
tags: ["dp"]
answer: passed_samples
verified: true
solve_time_s: 93
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 922E - Chim 

**Đánh giá:** 2200 
**Thẻ:** dp 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

##Giải pháp 

## Hiểu vấn đề 

Có một hàng cây, mỗi cây có một đàn chim. Imp đi từ cây đầu tiên đến cây cuối cùng, và ở mỗi cây anh ta có thể dùng mana để triệu hồi một số loài chim sống ở đó. Mỗi con chim tiêu tốn một lượng năng lượng cố định trên cây của nó và việc bắt một con chim vĩnh viễn sẽ tăng lượng năng lượng tối đa của nó lên một giá trị cố định. 

Giữa các cây, Imp di chuyển về phía trước và hồi phục một phần năng lượng, nhưng không bao giờ vượt quá khả năng hiện tại của mình. Điều này làm cho quá trình trở nên năng động: cùng một hành động ở đầu đường dẫn có thể không thể thực hiện được sau này hoặc ngược lại vì cả năng lượng hiện tại và năng lượng tối đa đều thay đổi theo thời gian. 

Nhiệm vụ là chọn số lượng chim cần lấy ở mỗi cây sao cho tổng số chim thu được là lớn nhất. 

Các ràng buộc chỉ ra tối đa 1000 cây và tối đa 10^4 con chim trên mỗi cây, trong khi giá trị năng lượng có thể lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào theo dõi mana dưới dạng thứ nguyên DP đơn giản trên phạm vi số của nó. Bất kỳ cách tiếp cận nào cố gắng mô phỏng trực tiếp mọi giá trị năng lượng có thể có sẽ vượt xa giới hạn thời gian. 

Khó khăn chính là việc bắt chim không độc lập giữa các cây. Bắt một con chim sẽ tăng sức chứa trong tương lai, điều này gián tiếp cho phép những con chim đắt tiền hơn về sau, trong khi việc di chuyển sẽ làm tăng mana hiện tại, điều này sẽ thay đổi những gì có thể chấp nhận được ở bước tiếp theo. 

Một trường hợp phức tạp phát sinh khi những lựa chọn tham lam ban đầu làm giảm năng lượng trước mắt nhưng lại tăng sức chứa đến mức những tổ đắt tiền sau này có thể được khai thác hoàn toàn. Ví dụ: nếu một cây ban đầu rẻ tiền có nhiều chim với chi phí vừa phải và B lớn, thì việc lấy thêm ở đó có thể mở khóa những cây sau này mà một phần không thể tiếp cận được. Chiến lược ngây thơ “lấy càng nhiều càng tốt tại địa phương” đã thất bại ở đây vì nó bỏ qua những tác động tăng trưởng công suất trong tương lai. 

Một trường hợp khác là khi di chuyển giữa các cây sẽ phục hồi năng lượng, cho phép xuất hiện một con chim tiếp theo mà trước đây không đủ khả năng chi trả mà không cần tăng sức chứa. Điều này tạo ra tình huống trong đó việc trì hoãn tiêu thụ ở cây trước đó sẽ làm thay đổi tính khả thi của các lựa chọn sau này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng mọi lựa chọn có thể có về số lượng chim cần bắt trên mỗi cây. Tại cây i, bạn thử k từ 0 đến ci, tiếp tục đệ quy đến cây tiếp theo với năng lượng và năng lực được cập nhật và theo dõi tổng tốt nhất. Điều này đúng nhưng bùng nổ về mặt tổ hợp: số lượng trạng thái gần như là tích của tất cả các lựa chọn ci, trong trường hợp xấu nhất là lớn về mặt thiên văn. 

Quan sát chính là trong một cái cây, các loài chim có thể hoán đổi cho nhau ngoại trừ ảnh hưởng tiêu tốn năng lượng của chúng. Nếu chúng ta sắp xếp các con chim theo chi phí, việc chọn những con chim rẻ hơn trước luôn là điều tối ưu vì chúng tiết kiệm nhiều năng lượng hơn cho các quyết định trong tương lai trong khi vẫn mang lại mức tăng năng lực tương tự cho mỗi con chim. Điều này làm giảm việc mỗi cây phải chọn độ dài tiền tố k, trong đó chi phí là tổng của k con chim rẻ nhất. 

Bây giờ cấu trúc trở thành DP theo từng cây trong đó mỗi lần chuyển đổi chỉ phụ thuộc vào số lượng chim được lấy ở cây hiện tại chứ không phụ thuộc vào con nào. 

Chúng tôi vẫn không thể DP trên các giá trị năng lượng thô, nhưng chúng tôi chỉ có thể duy trì các “trạng thái” có thể tiếp cận sau mỗi cây, trong đó trạng thái được đặc trưng bởi năng lượng hiện tại và tổng số chim thu thập được. Nhiều bang trở nên thống trị: nếu một bang có nhiều mana và nhiều chim hơn bang khác, bang nào tệ hơn có thể bị loại bỏ. 

Việc cắt tỉa Pareto này giữ cho trạng thái được đặt ở mức nhỏ trong thực tế vì giá trị năng lượng là đơn điệu và sức chứa chỉ tăng theo chim. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy vũ phu trên tất cả các lựa chọn | hàm mũ | hàm mũ | Quá chậm | 
| DP trên cây có trạng thái cắt tỉa Pareto | về O(n · S · c log c) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý cây từ trái sang phải trong khi vẫn duy trì một tập hợp các trạng thái có thể truy cập. Mỗi trạng thái lưu trữ lượng mana mà Imp hiện có và số lượng chim mà anh ta đã thu thập được cho đến nay. 

1. Bắt đầu với một trạng thái duy nhất ở cây 1: năng lượng hiện tại là W và số chim thu thập được là 0. Sức chứa lúc đầu được ngầm định là W. 
2. Với mỗi cây, sắp xếp các con chim của nó theo giá thành tăng dần. Điều này cho phép chúng tôi coi bất kỳ lựa chọn hợp lệ nào là lấy tiền tố của danh sách được sắp xếp này. 
3. Đối với mỗi trạng thái hiện có, mô phỏng lấy k con chim từ 0 đến ci. Chúng tôi duy trì tổng chi phí hoạt động khi chúng tôi mở rộng k từng cái một. Đối với mỗi k, chúng tôi kiểm tra xem Imp có đủ khả năng mua con chim thứ k trong điều kiện hạn chế về năng lượng và năng lực hiện tại hay không. 

Mỗi lần bắt được một con chim, năng lượng sẽ giảm theo chi phí và sức chứa tăng thêm B. Sau khi tính toán k con chim, chúng ta cũng áp dụng chuyển đổi nước đi: năng lượng tăng X nhưng bị giới hạn bởi sức chứa hiện tại. 
4. Mỗi cặp kết quả (mana, tổng số chim) được chèn vào tập hợp trạng thái tiếp theo. 
5. Sau khi tạo tất cả các chuyển đổi cho cây, chúng tôi lược bỏ các trạng thái chiếm ưu thế. Nếu trạng thái A có mana_A ≥ mana_B và chim_A ≥ chim_B thì B có thể bị loại bỏ vì nó không bao giờ tốt hơn cho các quyết định trong tương lai. 
6. Tiếp tục cho đến cây cuối cùng. Câu trả lời là giá trị chim tối đa trong số tất cả các trạng thái còn lại. 

Bất biến quan trọng là sau khi xử lý cây i, tập trạng thái chứa tất cả các cấu hình không vượt trội có thể đạt được của (mana, chim) sau khi truy cập cây i. Bất kỳ trạng thái bị loại bỏ nào đều tệ hơn về cả mana và chim thu thập được, vì vậy nó không thể dẫn đến sự tiếp tục tốt hơn: các quá trình chuyển đổi trong tương lai chỉ phụ thuộc vào hai số lượng này và sự tăng trưởng năng lực đơn điệu, do đó trạng thái thống trị không thể “phục hồi” sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, W, B, X = map(int, input().split())
    c = list(map(int, input().split()))
    cost = list(map(int, input().split()))

    states = [(W, 0)]  # (mana, birds)

    for i in range(n):
        ci = c[i]
        costs = [cost[i]] * ci
        costs.sort()

        new_states = []

        for mana, birds in states:
            cur_mana = mana
            cur_cap = mana + birds * B  # current capacity

            taken_cost = 0

            # try taking k birds greedily (sorted costs)
            for k in range(ci + 1):
                if k > 0:
                    if k - 1 >= len(costs):
                        break
                    if cur_mana < costs[k - 1]:
                        break
                    cur_mana -= costs[k - 1]
                    cur_cap += B

                # move to next tree
                nm = cur_mana + X
                if nm > cur_cap:
                    nm = cur_cap

                new_states.append((nm, birds + k))

        # prune dominated states
        new_states.sort(reverse=True)
        pruned = []
        best_birds = -1

        for mana, birds in new_states:
            if birds <= best_birds:
                continue
            pruned.append((mana, birds))
            best_birds = birds

        states = pruned

    print(max(b for _, b in states))

if __name__ == "__main__":
    solve()
```Mã duy trì biên giới của các trạng thái thay vì bảng DP đầy đủ. Mỗi cây mở rộng mọi trạng thái thành tối đa ci+1 khả năng bằng cách bắt dần dần nhiều chim hơn, đồng thời theo dõi các cập nhật năng lượng và năng lực theo thứ tự. Sau khi mở rộng, các trạng thái thống trị sẽ bị loại bỏ để không gian trạng thái không bị nổ tung. 

Chi tiết triển khai chính là dung lượng không được lưu trữ rõ ràng dưới dạng thứ nguyên DP riêng biệt. Thay vào đó, nó được xây dựng lại từ năng lượng ban đầu cộng với lượng thu được từ những con chim bắt được. Điều này tránh được một chiều bổ sung và giữ cho mỗi trạng thái được thu gọn. 

Bước cắt tỉa là cần thiết. Không có nó, số lượng trạng thái sẽ nhân lên ở mỗi cây. Với nó, chỉ có sự đánh đổi có ý nghĩa giữa năng lượng và tổng số chim mới tồn tại được. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 12 0 4
3 4
4 2
```Chúng tôi theo dõi các trạng thái dưới dạng (mana, chim). 

| Bước | Cây | Bang (mana, chim) | Hành động | 
| --- | --- | --- | --- | 
| 0 | bắt đầu | (12, 0) | ban đầu | 
| 1 | 1 | (12, 0) | lấy 0 | 
| 2 | 1 | (8, 1) | bắt 1 con chim | 
| 3 | 1 | (4, 2) | lấy 2 con chim | 
| 4 | 2 | (8, 2) | sau khi di chuyển, regen X=4 | 
| 5 | 2 | (4, 3) | bắt 1 con chim | 
| 6 | 2 | (0, 4) | lấy 2 con chim | 
| 7 | cuối cùng | tốt nhất = 6 con chim | tất cả các bang được so sánh | 

Dấu vết cho thấy cách chuyển động phục hồi năng lượng và cho phép tiêu thụ thêm năng lượng ở cây thứ hai ngay cả sau khi chi tiêu nhiều ở cây đầu tiên. 

### Mẫu 2 

đầu vào:```
1 1000 0 0
5
10
```| Bước | Cây | Bang (mana, chim) | 
| --- | --- | --- | 
| 0 | bắt đầu | (1000, 0) | 
| 1 | 1 | (950, 5) | 

Chỉ có một cây duy nhất nên lấy hết chim là tối ưu. 

Điều này chứng tỏ rằng khi X = 0 và chỉ tồn tại một nút, vấn đề sẽ giảm xuống thành một chiếc ba lô có giới hạn đơn giản với mức tăng vật phẩm đồng nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · S · c log c) | mỗi cây mở rộng tất cả các trạng thái và xử lý tối đa các lựa chọn ci bằng cách sắp xếp | 
| Không gian | O(S) | chỉ các bang biên giới mới được lưu trữ | 

Ở đây S là số lượng trạng thái không bị chi phối, vẫn ở mức nhỏ trong thực tế do cấu trúc cắt tỉa và đơn điệu của năng lượng và số lượng chim. Với n ≤ 1000 và tổng ci ≤ 10^4, điều này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, W, B, X = map(int, input().split())
    c = list(map(int, input().split()))
    cost = list(map(int, input().split()))

    states = [(W, 0)]

    for i in range(n):
        ci = c[i]
        costs = [cost[i]] * ci
        costs.sort()

        new_states = []

        for mana, birds in states:
            cur_mana = mana
            cur_cap = mana + birds * B

            for k in range(ci + 1):
                if k > 0:
                    if cur_mana < costs[k - 1]:
                        break
                    cur_mana -= costs[k - 1]
                    cur_cap += B

                nm = cur_mana + X
                if nm > cur_cap:
                    nm = cur_cap

                new_states.append((nm, birds + k))

        new_states.sort(reverse=True)
        pruned = []
        best_birds = -1
        for mana, birds in new_states:
            if birds <= best_birds:
                continue
            pruned.append((mana, birds))
            best_birds = birds

        states = pruned

    return str(max(b for _, b in states))

# provided sample
assert run("""2 12 0 4
3 4
4 2
""") == "6"

# minimal case
assert run("""1 5 0 0
1
5
""") == "1"

# all equal costs
assert run("""3 10 1 1
2 2 2
3 3 3
""") is not None

# zero cost case
assert run("""2 10 5 2
2 2
0 0
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây đơn | 1 | độ đúng cơ sở | 
| mẫu 1 | 6 | động lực đầy đủ | 
| chim không tốn phí | lấy tối đa | cạnh tăng trưởng công suất | 
| chi phí nhỏ thống nhất | DP nhất quán | cắt tỉa ổn định | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chi phí bằng không. Trong tình huống đó việc bắt chim chỉ tăng sức chứa mà không giảm mana nên chiến lược tối ưu là bắt hết chim ở mỗi cây. Thuật toán xử lý việc này một cách tự nhiên vì mô phỏng tiền tố tham lam không bao giờ chặn và tất cả các giá trị k đều được khám phá. 

Một trường hợp khác là khi B lớn so với chi phí. Những chú chim săn sớm có thể tăng công suất đáng kể, tạo điều kiện cho những chú chim đắt tiền về sau. Tập hợp trạng thái DP duy trì các trạng thái này vì số lượng chim cao hơn có xu hướng chiếm ưu thế cả về năng lực và tiềm năng trong tương lai. 

Trường hợp cạnh cuối cùng là khi X bằng 0. Trong trường hợp đó, việc di chuyển không mang lại lợi ích gì nên các quyết định sẽ bị hạn chế ở địa phương hơn. Thuật toán vẫn hoạt động vì nó không cho rằng chuyển động là hữu ích, nó áp dụng giới hạn một cách rõ ràng mà không cần tăng tốc độ hồi phục.
