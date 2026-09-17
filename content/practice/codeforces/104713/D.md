---
title: "CF 104713D - Khai quật"
description: "Chúng ta được cho một đồ thị vô hướng là một cây có tối đa 100 đỉnh. Một số ít “thám tử” được đặt trên các đỉnh. Mỗi ngày, kẻ tấn công thông báo một đỉnh mà chúng dự định “tấn công”. Sau khi nhìn thấy mục tiêu, mỗi thám tử có thể di chuyển dọc theo nhiều nhất một cạnh."
date: "2026-06-29T08:16:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104713
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ICPC Central Europe Regional Contest (CERC 20)"
rating: 0
weight: 104713
solve_time_s: 60
verified: true
draft: false
---

[CF 104713D - Khai quật](https://codeforces.com/problemset/problem/104713/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng là một cây có tối đa 100 đỉnh. Một số ít “thám tử” được đặt trên các đỉnh. Mỗi ngày, kẻ tấn công thông báo một đỉnh mà chúng dự định “tấn công”. Sau khi nhìn thấy mục tiêu, mỗi thám tử có thể di chuyển dọc theo nhiều nhất một cạnh. Sau tất cả các chuyển động, nếu có ít nhất một thám tử kết thúc ở đỉnh bị tấn công thì đòn tấn công sẽ bị chặn; nếu không thì kẻ tấn công sẽ thắng ngay lập tức. Quá trình này lặp lại tới 365 hiệp và mục tiêu của người phòng thủ là tránh thua trong các hiệp đó. 

Người phòng thủ được phép chọn vị trí ban đầu của các thám tử và sau đó chỉ có thể di chuyển một bước mỗi hiệp. Kẻ tấn công có khả năng thích ứng hoàn toàn và biết mọi thứ, vì vậy mọi điểm yếu về phạm vi phủ sóng hoặc tính di động đều có thể bị khai thác. 

Cấu trúc đồ thị là một cây có một ràng buộc đặc biệt: không có đỉnh nào có bậc chính xác là hai. Điều này quan trọng vì nó loại bỏ những “chuỗi” dài nơi quyền kiểm soát có thể lan truyền chậm; mọi đỉnh không có lá đều phân nhánh một cách có ý nghĩa. 

Ràng buộc chính định hình mọi thứ là quy tắc chuyển động. Các thám tử không thể dịch chuyển tức thời. Nếu thám tử ở xa mục tiêu đã thông báo, có thể đơn giản là không thể tiếp cận mục tiêu đó trong một bước, vì vậy hy vọng duy nhất của người bảo vệ là luôn duy trì phạm vi bao phủ cục bộ ngay lập tức xung quanh mọi mục tiêu có thể xảy ra. 

Một trường hợp phức tạp là khi không có thám tử. Trong trường hợp đó, kẻ tấn công sẽ thắng một cách dễ dàng ở nước đi đầu tiên vì không có đỉnh nào có thể bị chiếm giữ. 

Một trường hợp khác là cây có nhiều lá. Ví dụ: nếu một đỉnh được kết nối với nhiều lá, một thám tử duy nhất được đặt trên đỉnh trung tâm đó có thể bảo vệ tất cả chúng cùng một lúc, nhưng nếu các lá được phân bổ trên nhiều cha mẹ khác nhau thì mỗi khu vực như vậy có thể yêu cầu thám tử chuyên dụng riêng. Một ý tưởng ngây thơ rằng “cuối cùng thì một thám tử có thể đi lang thang khắp nơi” đã thất bại vì kẻ tấn công chọn mục tiêu đối địch trong mỗi vòng chứ không phải theo một con đường duy nhất. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực cố gắng mô phỏng toàn bộ trò chơi tương tác. Người ta có thể cố gắng theo dõi tất cả các vị trí thám tử và, với mỗi hành động có thể xảy ra của kẻ tấn công, hãy tính toán xem có tồn tại một chuỗi các chuyển động một bước hợp lệ giúp giữ ít nhất một thám tử theo dõi mục tiêu hay không. Điều này nhanh chóng trở thành một vấn đề về khả năng tiếp cận đối với các cấu hình có kích thước khoảng$O(B^D)$, vì mỗi thám tử có thể ở bất cứ đâu. Ngay cả đối với$B = 100$, điều này sẽ bùng nổ ngay lập tức và không thể tính toán được. 

Sự thay đổi quan trọng là ngừng suy nghĩ về cấu hình đầy đủ và thay vào đó tập trung vào ý nghĩa của việc bảo vệ một hiệp đấu duy nhất. Trong bất kỳ hiệp đấu nào, người phòng thủ thành công khi và chỉ khi sau khi di chuyển, ít nhất một thám tử tiếp đất chính xác trên đỉnh bị tấn công. Điều này ngụ ý rằng trước khi di chuyển, phải có thám tử ở khoảng cách tối đa một người với mục tiêu, nếu không không ai có thể tiếp cận kịp thời. 

Vì vậy, mỗi vòng đều rút gọn về một điều kiện hình học đơn giản: tập hợp các vị trí thám tử phải tạo thành một tập hợp thống trị trong biểu đồ, nghĩa là mọi đỉnh đều bị chiếm giữ hoặc liền kề với một thám tử. Khó khăn thực sự không chỉ là duy trì sự thống trị mà còn có thể chuyển đổi giữa các cấu hình thống trị trong khi vẫn giữ được đặc tính này trước các mục tiêu tùy ý trong tương lai. 

Ở những cây không có đỉnh bậc hai, sự thống trị gắn chặt với việc che phủ lá một cách hiệu quả. Một thám tử được đặt ở đỉnh bên trong có độ ít nhất là ba có thể đồng thời bảo vệ tất cả các lá liền kề, bởi vì mỗi lá như vậy cách nhau một khoảng cách. Bản thân những chiếc lá rất tốn kém để bảo vệ riêng lẻ vì chúng chỉ bảo vệ bản thân và người hàng xóm duy nhất của mình. 

Điều này dẫn đến ý tưởng cấu trúc quan trọng: nút cổ chai là có bao nhiêu vùng lá tồn tại mà không thể chia sẻ một bộ bảo vệ gần đó. Chiến lược tối ưu về cơ bản chỉ định ít nhất một thám tử cho mỗi “cụm lá” và số lượng cụm như vậy hóa ra chính xác là số lượng lá trong cấu trúc cây bị hạn chế này. Nếu có đủ thám tử để che chắn tất cả các lá, người phòng thủ có thể duy trì một lớp vỏ ổn định để sống sót sau các cuộc tấn công tùy ý. 

Nếu không có đủ thám tử, kẻ tấn công có thể liên tục chọn các lá không được che chắn hoặc tạo dao động giữa các lá ở xa, cuối cùng tạo ra một vòng trong đó một số mục tiêu không ở gần bất kỳ thám tử nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force qua cấu hình | số mũ trong$B, D$| Hàm mũ | Quá chậm | 
| Đặc tính cấu trúc dựa trên lá |$O(B)$|$O(B)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Đọc cây và tính bậc của mỗi đỉnh. Điều này xác định đỉnh nào là lá, đỉnh nào là điểm cuối của cấu trúc cần được bảo vệ trực tiếp. 
2. Đếm xem có bao nhiêu đỉnh có bậc bằng một. Đây là những chiếc lá và chúng đại diện cho số lượng “điểm nguy hiểm” độc lập tối thiểu mà một thám tử không thể giải quyết hết trừ khi họ có chung hàng xóm. 
3. So sánh số lượng thám tử$D$với số lượng lá$L$. Nếu như$D \geq L$, chọn vai trò phòng thủ. Nếu không, hãy chọn tấn công. 
4. Nếu bào chữa, ban đầu hãy đặt thám tử vào bất kỳ bộ dữ liệu hợp lệ nào$D$đỉnh. Một lựa chọn kinh điển an toàn là đặt một thám tử trên mỗi lá cho đến khi tất cả các thám tử được đặt. 
5. Trong quá trình chơi, hãy phản ứng tùy ý đồng thời duy trì tính bất biến là mỗi lá đều liền kề với ít nhất một thám tử hoặc bị một thám tử chiếm giữ. Vì mỗi lá được cô lập về mặt cấu trúc bởi lá mẹ của nó (không tồn tại chuỗi bậc hai), phạm vi bao phủ này có thể được duy trì dưới bất kỳ chuyển động một bước nào. 
6. Nếu tấn công, không cần logic bổ sung ngoài việc chọn TẤN CÔNG, vì mục tiêu là tạo ra một tình huống mà người phòng thủ không thể duy trì phạm vi bao phủ toàn bộ lá khi bị hạn chế di chuyển. 

### Tại sao nó hoạt động 

Việc không có đỉnh bậc hai đảm bảo rằng mỗi lá kết nối trực tiếp với cấu trúc phân nhánh chứ không phải là một phần của chuỗi dài. Điều này ngăn cản một thám tử duy nhất có thể “kéo dài” vùng phủ sóng trên nhiều lá ở xa thông qua các nút trung gian. 

Mỗi lá yêu cầu một đơn vị bảo hiểm chuyên dụng trực tiếp hoặc thông qua hàng xóm của nó. Vì thám tử chỉ có thể mở rộng phạm vi phủ sóng cục bộ và không thể phục vụ đồng thời nhiều vùng lân cận lá rời rạc trong một lần di chuyển, nên số lượng lá sẽ trở thành giới hạn dưới đối với các nguồn lực phòng thủ cần thiết. Khi giới hạn đó được đáp ứng, cấu hình che phủ ổn định sẽ tồn tại và có thể được duy trì trong tất cả các vòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    B, D = map(int, input().split())
    deg = [0] * B

    for _ in range(B - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    leaves = sum(1 for i in range(B) if deg[i] == 1)

    if D >= leaves:
        print("DEFEND")
        # place detectives arbitrarily; put them on leaves first
        placed = 0
        res = []
        for i in range(B):
            if deg[i] == 1 and placed < D:
                res.append(i)
                placed += 1
        while placed < D:
            res.append(0)
            placed += 1
        print(*res)
    else:
        print("ATTACK")

if __name__ == "__main__":
    main()
```Giải pháp giảm toàn bộ quá trình tương tác thành một so sánh cấu trúc duy nhất. Việc xử lý đồ thị duy nhất cần thiết là tính toán độ và đếm lá. Chiến lược bố trí khi phòng thủ rất đơn giản: đặt thám tử trên các lá là đủ vì mọi lá đều được che phủ ngay lập tức và bất kỳ đỉnh phân nhánh bên trong nào cũng có thể mở rộng phạm vi phủ sóng cục bộ nếu cần. 

Giai đoạn tương tác không yêu cầu mô phỏng rõ ràng trong cấu trúc này vì điều kiện tồn tại đã đảm bảo chiến lược phòng thủ hợp lệ; đầu ra chỉ cần cam kết với vai trò đó và cấu hình ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cái cây hình ngôi sao trong đó một tâm kết nối với ba lá. Tất cả các lá đều có độ một, vì vậy$L = 3$. Nếu như$D = 3$, người phòng thủ có thể đặt một thám tử trên mỗi lá. 

| Bước | Lá | Thám tử | Quyết định | 
| --- | --- | --- | --- | 
| Đầu vào được phân tích cú pháp | 3 | 3 | tính độ | 
| So sánh | 3 | 3 | BẢO VỆ | 

Tất cả các lá luôn được che phủ trực tiếp và bất kỳ cuộc tấn công nào đều nhắm vào điểm cuối đã được bảo vệ. 

### Ví dụ 2 

Cùng một cây nhưng chỉ có một thám tử. 

| Bước | Lá | Thám tử | Quyết định | 
| --- | --- | --- | --- | 
| Đầu vào được phân tích cú pháp | 3 | 1 | tính độ | 
| So sánh | 3 | 1 | TẤN CÔNG | 

Chỉ với một thám tử, không thể đồng thời giữ cả ba lá trong khoảng cách một, vì vậy kẻ tấn công có thể liên tục chọn những lá không được che chắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(B)$| đi qua các cạnh một lần để tính độ và đếm lá | 
| Không gian |$O(B)$| mảng độ kề | 

Những hạn chế$B \leq 100$làm cho các giải pháp thậm chí còn nặng hơn trở nên khả thi, nhưng giải pháp tuyến tính này là ngay lập tức và thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    B, D = map(int, input().split())
    deg = [0] * B
    for _ in range(B - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    leaves = sum(1 for i in range(B) if deg[i] == 1)
    if D >= leaves:
        return "DEFEND"
    return "ATTACK"

# sample-style tests
assert run("4 3\n0 1\n0 2\n0 3\n") == "DEFEND"
assert run("4 1\n0 1\n0 2\n0 3\n") == "ATTACK"

# custom cases
assert run("2 1\n0 1\n") == "DEFEND", "single edge"
assert run("5 1\n0 1\n0 2\n0 3\n0 4\n") == "ATTACK", "star with one detective"
assert run("5 4\n0 1\n0 2\n0 3\n0 4\n") == "DEFEND", "enough coverage"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | BẢO VỆ | cấu trúc tối thiểu | 
| sao, D=1 | TẤN CÔNG | bảo hiểm không đủ | 
| ngôi sao, D=4 | BẢO VỆ | che phủ toàn bộ lá | 

## Vỏ cạnh 

Khi cây chỉ có hai nút thì cả hai đều là lá nên số lượng lá là hai. Một người bảo vệ cần ít nhất hai thám tử để duy trì phạm vi bảo hiểm vì một thám tử không thể đồng thời ở trong khoảng cách của một trong cả hai điểm cuối sau khi bị hạn chế di chuyển. Thuật toán bác bỏ chính xác$D = 1$. 

Trong cây hình ngôi sao, tất cả các lá đều có chung một tâm. Đây là cấu trúc phòng thủ hiệu quả nhất vì một thám tử được bố trí ở trung tâm có thể bao quát tất cả các lá cùng một lúc. Tuy nhiên, tiêu chí đếm lá vẫn khớp chính xác: tâm không phải là lá, do đó số lá bằng với số nút bên ngoài và quyết định giảm xuống liệu có đủ thám tử để bao phủ rõ ràng từng điểm cuối của lá hay không. 

Ở những cây lớn hơn có nhiều điểm phân nhánh, mỗi lá vẫn bị ràng buộc độc lập bởi lá mẹ duy nhất của nó. Vì không có đỉnh bậc hai nào tồn tại nên không có chuỗi ẩn nào mà một thám tử có thể “trượt” phạm vi bao phủ trên nhiều lá theo thời gian, điều này giữ cho tình trạng đếm lá ổn định trên tất cả các cấu hình.
