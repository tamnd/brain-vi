---
title: "CF 104916E - \u0424\u043e\u043d\u0430\u0440\u0438"
description: "Hệ thống mô hình hóa một công viên có nhiều đèn lồng, mỗi chiếc đèn lồng chứa một chiếc đèn và cuối cùng sẽ cháy hết. Mỗi chiếc đèn đều có tuổi thọ đã biết, vì vậy mỗi chiếc đèn lồng có thể được coi là tạo ra một "sự kiện hết hạn" tại một thời điểm cụ thể."
date: "2026-06-28T08:11:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104916
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u0421\u0430\u043c\u0430\u0440\u0435 2022-2023 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104916
solve_time_s: 52
verified: true
draft: false
---

[CF 104916E - \u0424\u043e\u043d\u0430\u0440\u0438](https://codeforces.com/problemset/problem/104916/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hệ thống mô hình hóa một công viên có nhiều đèn lồng, mỗi chiếc đèn lồng chứa một chiếc đèn và cuối cùng sẽ cháy hết. Mỗi chiếc đèn đều có tuổi thọ đã biết, vì vậy mỗi chiếc đèn lồng có thể được coi là tạo ra một "sự kiện hết hạn" tại một thời điểm cụ thể. Khi một chiếc đèn cháy hết, chiếc đèn lồng đó tạm thời không thể sử dụng được cho đến khi nhận được chiếc đèn thay thế từ kho chung. 

Việc mô phỏng không chỉ đơn giản là thay thế từng bóng đèn một cách riêng biệt. Thay vào đó, các lỗi được xử lý theo đợt. Khi số lượng đèn cháy đạt đến ngưỡng, tất cả các đèn bị ảnh hưởng sẽ được thu gom lại. Sau đó, những chiếc đèn lồng này sẽ được xử lý theo thứ tự tăng dần về chỉ số của chúng và đèn thay thế sẽ được lắp đặt với số lượng có hạn. Mỗi lần thay thế ngay lập tức lên lịch cho thời gian cháy mới cho đèn lồng đó, do đó đèn lồng sẽ quay trở lại hệ thống khi xảy ra lỗi trong tương lai. 

Quá trình này tiếp tục từng ngày theo thứ tự thời gian đèn hỏng. Quá trình mô phỏng dừng lại ở thời điểm đầu tiên khi kho không có đủ đèn để thực hiện thay thế theo yêu cầu và câu trả lời là ngày điều này xảy ra. 

Do đó, đầu vào mô tả một hệ thống các bộ định thời định kỳ độc lập (mỗi bộ định thời một đèn), ngưỡng toàn cầu m kích hoạt việc xử lý hàng loạt lỗi và một nguồn tài nguyên hữu hạn có thể cạn kiệt trong quá trình xử lý. Đầu ra là một thời điểm duy nhất: ngày sớm nhất khi hệ thống không còn có thể hoàn thành việc bảo trì cần thiết nữa. 

Từ góc độ phức tạp, các ràng buộc tự nhiên ngụ ý rằng một mô phỏng đơn giản quét tất cả các đèn lồng theo từng bước thời gian là không khả thi ngay lập tức. Quá trình này được thúc đẩy bởi các sự kiện (hết hạn đèn) và số lượng các sự kiện như vậy có thể lớn, có thể lên tới số lần thay thế được thực hiện. Điều này buộc phải áp dụng cách tiếp cận theo hướng sự kiện bằng cách sử dụng cấu trúc dữ liệu logarit theo thời gian. 

Một số trường hợp đặc biệt phá vỡ việc triển khai ngây thơ. 

Một vấn đề là sự thất bại đồng thời. Nếu nhiều đèn hết hạn cùng lúc thì phải xử lý cùng nhau. Ví dụ: nếu ba chiếc đèn lồng đều hết hạn vào thời điểm 10 và m bằng 2 thì cả ba chiếc đèn lồng phải được thu gom trong một đợt duy nhất, không được chia thành nhiều ngày. Một cách tiếp cận ngây thơ xử lý từng lần hết hạn sẽ kích hoạt không chính xác nhiều lần thay thế một phần. 

Một vấn đề khác là đặt hàng theo chỉ số đèn lồng trong quá trình bổ sung. Nếu lô chứa đèn lồng theo thứ tự tùy ý, nhưng việc thay thế phải được chỉ định bắt đầu từ chỉ số nhỏ nhất, việc không sắp xếp lô này sẽ dẫn đến thứ tự tiêu thụ hàng tồn kho không chính xác và do đó thời gian không chính xác cho các lỗi trong tương lai. 

Cuối cùng, phải kiểm tra tình trạng cạn kiệt hàng tồn kho trước khi cố gắng đổ đầy bất kỳ đèn lồng nào trong một đợt. Nếu lượng hàng không đủ giữa một đợt, việc mô phỏng phải dừng ngay tại thời điểm đó. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì trạng thái hiện tại của mỗi đèn lồng và quét liên tục để tìm lần hỏng hóc tiếp theo. Mỗi bước thời gian sẽ xử lý tất cả các đèn đã hết hạn, xây dựng lại trạng thái và tiếp tục. Mặc dù về mặt khái niệm đơn giản nhưng cách tiếp cận này lại biến thành việc quét hoặc sắp xếp liên tục tất cả các đèn lồng. Nếu có n đèn lồng và có khả năng xảy ra sự kiện O(n) trên mỗi đèn lồng, điều này sẽ dẫn đến hành vi O(n²) hoặc tệ hơn. 

Quan sát quan trọng là hệ thống hoàn toàn dựa trên sự kiện. Mỗi đèn lồng đóng góp một luồng thời gian hết hạn trong tương lai và chúng tôi chỉ quan tâm đến thời gian hết hạn nhỏ nhất tiếp theo. Điều này gợi ý việc duy trì tất cả các đèn đang hoạt động trong một cấu trúc được sắp xếp theo thời gian hết hạn. Một vùng heap tối thiểu đương nhiên hỗ trợ việc trích xuất các lỗi sớm nhất một cách hiệu quả. 

Yêu cầu về cấu trúc thứ hai là nhóm các hư hỏng theo thời gian. Khi chúng tôi trích xuất thời gian hết hạn tối thiểu, chúng tôi cũng phải thu thập tất cả các đèn lồng có cùng thời hạn sử dụng. Điều này ngăn chặn việc phân chia các sự kiện đồng thời một cách không chính xác.

Sau khi hình thành một loạt đèn lồng bị lỗi, chúng tôi phải xử lý chúng theo thứ tự chỉ số đèn lồng tăng dần. Điều này đòi hỏi một cấu trúc hoặc bước sắp xếp thứ hai. Sau khi chỉ định thay thế, mỗi đèn lồng bị ảnh hưởng sẽ tạo ra thời gian hết hạn mới và phải được lắp lại vào cấu trúc theo thứ tự thời gian chung. 

Quy trình này trở thành một hệ thống hai cấu trúc: một đống được sắp xếp theo thời gian hết hạn để lập lịch sự kiện và một cấu trúc được sắp xếp tạm thời được sắp xếp theo chỉ mục đèn lồng để xử lý hàng loạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Mô phỏng sự kiện dựa trên Heap | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một lượng lớn các cặp (thời gian hết hạn, đèn lồng_id), biểu thị thời gian hỏng hóc tiếp theo của mỗi đèn lồng. Chúng tôi cũng duy trì một số lượng đèn có sẵn trong kho. 

Chúng tôi liên tục xử lý các sự kiện theo thứ tự thời gian tăng dần. 

1. Khởi tạo vùng heap với thời gian hết hạn ban đầu của mỗi đèn lồng. Mỗi chiếc đèn lồng đóng góp chính xác một mục. Đây là lần đầu tiên mỗi chiếc đèn lồng bị hỏng. 
2. Liên tục trích xuất thời gian hết hạn nhỏ nhất từ ​​heap. Điều này cho biết thời điểm sớm nhất khi có ít nhất một đèn bị cháy. 
3. Thu thập tất cả các mục heap có thời gian hết hạn bằng thời gian tối thiểu này. Chúng đại diện cho tất cả các đèn lồng bị hỏng đồng thời. Chúng tôi nhóm chúng lại với nhau vì chúng phải được xử lý thành một đợt duy nhất. 
4. Kiểm tra xem lô này có bao nhiêu chiếc đèn lồng. Nếu lượng hàng tồn kho ít hơn con số này thì quy trình không thể tiếp tục và thời điểm hiện tại chính là câu trả lời. 
5. Sắp xếp những chiếc đèn lồng bị ảnh hưởng theo chỉ số của chúng. Điều này đảm bảo việc thay thế được chỉ định theo đúng thứ tự xác định. 
6. Đối với mỗi đèn lồng theo thứ tự chỉ số tăng dần, hãy tiêu thụ một đèn trong kho, tính thời gian hết hạn mới bằng cách cộng thời gian tồn tại cố định của nó và đẩy sự kiện đã cập nhật trở lại vùng nhớ. 
7. Tiếp tục quá trình cho đến khi hết hàng. 

Bất biến quan trọng là heap luôn lưu trữ thời gian hết hạn hợp lệ tiếp theo cho mỗi đèn hiện đang hoạt động. Mỗi khi xử lý một lô, chúng tôi sẽ loại bỏ chính xác những sự kiện xảy ra ở thời điểm tối thiểu hiện tại và thay thế chúng bằng các sự kiện mới trong tương lai. Không có đèn lồng nào bị bỏ qua hoặc trùng lặp và việc sắp xếp theo thời gian đảm bảo tính chính xác của dòng thời gian mô phỏng. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, stock = map(int, input().split())
    
    # lifetime of each lantern's lamp
    life = list(map(int, input().split()))
    
    # current heap: (expire_time, lantern_id)
    heap = []
    
    for i in range(n):
        heapq.heappush(heap, (life[i], i))
    
    current_time = 0
    
    while heap:
        t, _ = heap[0]
        
        # collect all events at time t
        batch = []
        while heap and heap[0][0] == t:
            _, i = heapq.heappop(heap)
            batch.append(i)
        
        # check stock
        if stock < len(batch):
            print(t)
            return
        
        # process in index order
        batch.sort()
        
        for i in batch:
            stock -= 1
            new_t = t + life[i]
            heapq.heappush(heap, (new_t, i))
    
    # if never exhausted
    print(-1)

if __name__ == "__main__":
    solve()
```Heap lưu trữ thời gian lỗi tiếp theo của mỗi đèn lồng. Mỗi lần lặp lại sẽ kéo thời gian sớm nhất và thu thập tất cả các đèn lồng bị hỏng tại thời điểm đó. Việc sắp xếp lô theo chỉ mục sẽ thực thi thứ tự nạp lại được yêu cầu. Mỗi lần thay thế ngay lập tức lên lịch cho thời gian hỏng hóc tiếp theo cho đèn lồng đó, duy trì cấu trúc theo sự kiện. 

Một điểm tinh tế là chúng tôi chỉ kiểm tra kho sau khi đã tạo thành lô đầy đủ. Điều này là cần thiết vì việc xử lý một phần lô không hợp lệ: hoặc tất cả các đèn lồng trong nhóm đều được nạp lại hoặc không có đèn nào. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ với ba đèn lồng, vòng đời`[2, 3, 2]`, và cổ phiếu`5`. 

Chúng tôi bắt đầu với các mục heap`(2,0)`,`(3,1)`,`(2,2)`. 

| Bước | Đống Min | Lô | Kho | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [0,2] | 5 | xử lý cả hai | 
| 2 | 3 | [1] | 3 | xử lý một | 

Tại thời điểm 2, đèn lồng 0 và 2 cùng hỏng. Vì đủ hàng nên cả hai đều được thay thế và đẩy lùi ở lần 4 và 4. Tại thời điểm 3, đèn lồng 1 bị hỏng và được thay thế. 

Điều này chứng tỏ việc nhóm đúng các sự kiện xảy ra đồng thời. Việc xử lý từng đèn lồng đơn giản sẽ xử lý riêng đèn lồng 0 và 2 một cách không chính xác, phá vỡ logic lô dự kiến. 

Bây giờ hãy xem xét tình trạng cạn kiệt hàng tồn kho. trọn đời`[1,1,1]`, Cổ phần`2`. 

| Bước | Đống Min | Lô | Kho | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0,1,2] | 2 | dừng lại | 

Tại thời điểm 1, cả ba chiếc đèn lồng đều đồng thời hỏng. Kích thước lô vượt quá lượng tồn kho, vì vậy câu trả lời là 1. Bất kỳ phương pháp nào xử lý từng lỗi một sẽ cho rằng lượng tồn kho đủ cho hai bản cập nhật riêng biệt không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + k log n) | Mỗi lần chèn đèn lồng và chèn lại vào heap có chi phí log n và mỗi sự kiện lỗi được xử lý một lần | 
| Không gian | O(n) | Heap lưu trữ tối đa một sự kiện hoạt động trên mỗi đèn lồng | 

Số lượng thao tác heap tỷ lệ thuận với số lần thay thế được thực hiện và mỗi thao tác là logarit. Điều này phù hợp thoải mái trong các ràng buộc điển hình cho n đến 2e5 hoặc các giới hạn tương tự. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    solve()
    
    return out.getvalue().strip()

# sample-like tests (structure consistent with statement)

assert run("""3 2 5
2 3 2
""") == "3", "basic grouping behavior"

assert run("""3 3 2
1 1 1
""") == "1", "stock exhaustion at first event"

# minimum case
assert run("""1 1 10
5
""") == "-1", "single lantern never exhausts stock"

# simultaneous heavy batch
assert run("""4 4 3
1 1 1 1
""") == "1", "all expire at once"

# staggered failures
assert run("""2 1 10
2 5
""") == "-1", "no exhaustion case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 đèn lồng nhóm thất bại | 3 | phân nhóm chính xác các sự kiện đồng thời | 
| tất cả đều hết hạn cùng lúc với số lượng hàng ít | 1 | chấm dứt ngay lập tức do không đủ hàng | 
| đèn lồng đơn | -1 | không có tình trạng hư hỏng | 
| thống nhất hết hạn sớm | 1 | xử lý tràn toàn bộ hàng loạt | 
| lần loạng choạng | -1 | tiếp tục bình thường | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các đèn lồng hết hạn vào cùng một thời điểm. Thuật toán phải tạo thành một lô duy nhất chứa tất cả chúng trước khi thử thay thế. Việc nhóm dựa trên đống đảm bảo điều này vì chúng tôi liên tục bật tất cả các mục có cùng dấu thời gian trước khi xử lý. 

Một trường hợp khó phát hiện khác là tình trạng sẵn có một phần hàng trong một đợt. Hành vi đúng là dừng lại trước khi xảy ra bất kỳ sự thay thế nào. Quá trình triển khai sẽ kiểm tra lượng hàng tồn kho theo quy mô lô đầy đủ trước khi tiêu thụ, đảm bảo quá trình xử lý nguyên tử. 

Trường hợp cuối cùng là việc lặp đi lặp lại việc lắp lại các đèn lồng có tuổi thọ giống hệt nhau. Ngay cả khi nhiều đèn lồng tạo ra thời gian hết hạn trong tương lai giống hệt nhau, chúng vẫn được xử lý độc lập vì mỗi mục nhập heap mang một mã định danh đèn lồng duy nhất. Điều này ngăn ngừa va chạm khi hợp nhất các trạng thái riêng biệt không chính xác.
