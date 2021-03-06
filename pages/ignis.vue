<template>
  <main class="element ignis-element">
    <ElementPresentation
      class="element__presentation"
      :title="title"
      logo="/images/qiskit-ignis-logo.svg"
      :description="description"
      sources="https://github.com/Qiskit/qiskit-ignis"
      segment-action="Qiskit Ignis: GitHub Repository"
    />
    <PageSection class="element__body" framed>
      <article class="element__copy">
        <h2 class="element__header">
          About
        </h2>
        <p class="element__paragraph">
          Qiskit Ignis is a framework for understanding and mitigating noise in
          quantum circuits and systems. The experiments provided in Ignis are
          grouped into the topics of characterization, verification and
          mitigation. Characterization experiments are designed to measure
          noise parameters in the system. Verification experiments are designed
          to verify gate and small circuit performance. Mitigation experiments
          run calibration circuits that are analysed to generate mitigation
          routines that can be applied to arbitrary sets of results run on the
          same backend.
        </p>
        <h2 class="element__header">
          Stack
        </h2>
        <SoftwareStack :stack="elementStack" />
      </article>
      <template #aside>
        <article class="element__example">
          <h2 class="element__header">
            Example
          </h2>
          <SyntaxHighlight
            :label="title"
            :code="codeExample"
          />
        </article>
      </template>
    </PageSection>
  </main>
</template>

<script lang="ts">
import { Component } from 'vue-property-decorator'
import QiskitElementPage from '~/components/logic/QiskitElementPage.vue'
import ElementPresentation from '~/components/elements/ElementPresentation.vue'
import PageSection from '~/components/ui/PageSection.vue'
import SoftwareStack, { StackLayer } from '~/components/ui/SoftwareStack.vue'
import SyntaxHighlight from '~/components/ui/SyntaxHighlight.vue'
import AppCta from '~/components/ui/AppCta.vue'

@Component({
  components: {
    AppCta,
    ElementPresentation,
    PageSection,
    SoftwareStack,
    SyntaxHighlight
  },
  head (this: QiskitElementPage) {
    return {
      title: `${this.title} | ${this.description}`,
      link: [
        { rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.9.0/styles/atom-one-dark.min.css' }
      ]
    }
  }
})
export default class extends QiskitElementPage {
  title = 'Qiskit Ignis'
  description = 'Understanding and mitigating noise in quantum systems.'
  routeName = 'ignis-element'
  elementStack: Array<StackLayer> = [
    {
      title: 'Qiskit Ignis Experiments',
      description: 'List of Quantum Circuits or Pulse Schedules'
    },
    {
      title: 'Qiskit Terra',
      description: 'Compile Circuits or Schedules'
    },
    {
      title: 'Providers',
      description: 'Qiskit Aer, IBM Quantum, Third Party'
    },
    {
      title: 'Fitter/Filter',
      description: 'Fit to a Model/Plot Results'
    }
  ]

  codeExample = `import qiskit
from qiskit.providers.aer.noise import NoiseModel
from qiskit.providers.aer.noise.errors.standard_errors import depolarizing_error

# Import the RB Functions
from qiskit.ignis.verification.randomized_benchmarking import randomized_benchmarking_seq, RBFitter

# Generate RB circuits (2Q RB)
rb_opts = {}
rb_opts['length_vector'] = [1, 10, 20, 50, 75, 100, 125]
rb_opts['nseeds'] = 5
rb_opts['rb_pattern'] = [[0, 1]]
rb_circs, xdata = randomized_benchmarking_seq(**rb_opts)

# Run on a noisy simulator
noise_model = NoiseModel()
noise_model.add_all_qubit_quantum_error(depolarizing_error(0.002, 1), ['u1', 'u2', 'u3'])
noise_model.add_all_qubit_quantum_error(depolarizing_error(0.002, 2), 'cx')

backend = qiskit.Aer.get_backend('qasm_simulator')

# Create the RB fitter
rb_fit = RBFitter(None, xdata, rb_opts['rb_pattern'])
for rb_seed,rb_circ_seed in enumerate(rb_circs):

    job = qiskit.execute(rb_circ_seed, backend=backend,
         basis_gates=['u1','u2','u3','cx'],
         noise_model=noise_model)

    # Add data to the fitter
    rb_fit.add_data(job.result())
    print('After seed %d, EPC %f'%(rb_seed,rb_fit.fit[0]['epc']))`
}
</script>

<style lang="scss" scoped>
@import '~/assets/scss/element.scss';

.ignis-element {
  --community-header__background-color: rgb(255, 207, 225);
  --community-header__text-color: $inverse-01;
}
</style>
