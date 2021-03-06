Agentes Temporais
=================

Em aplicações reais é comum que o comportamento do agente seja executado de tempos em tempos e não apenas uma vez, mas como fazer isso no Pade? :(

Execução de um agente temporal
------------------------------

Este é um exemplo de um agente que executa indefinidamente um comportamento a cada 1,0 segundos. O código fonte deste agente de comportamento temporal pode ser encontrado no diretório de exemplos no repositório do PADE no GitHub, no arquivo agent_example_2.py.

::

    #!coding=utf-8
    # Hello world temporal in Pade!

    from pade.misc.utility import display_message, start_loop
    from pade.core.agent import Agent
    from pade.acl.aid import AID
    from pade.behaviours.protocols import TimedBehaviour
    from sys import argv

    class ComportTemporal(TimedBehaviour):
        def __init__(self, agent, time):
            super(ComportTemporal, self).__init__(agent, time)

        def on_time(self):
            super(ComportTemporal, self).on_time()
            display_message(self.agent.aid.localname, 'Hello World!')


    class AgenteHelloWorld(Agent):
        def __init__(self, aid):
            super(AgenteHelloWorld, self).__init__(aid=aid, debug=False)

            comp_temp = ComportTemporal(self, 1.0)

            self.behaviours.append(comp_temp)


    if __name__ == '__main__':
        agents_per_process = 2
        c = 0
        agents = list()
        for i in range(agents_per_process):
            port = int(argv[1]) + c
            agent_name = 'agent_hello_{}@localhost:{}'.format(port, port)
            agente_hello = AgenteHelloWorld(AID(name=agent_name))
            agents.append(agente_hello)
            c += 1000
        
        start_loop(agents)
